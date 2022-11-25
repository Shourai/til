# Running Systemd Service as User Rather Than Root

https://askubuntu.com/questions/1371102/running-systemd-service-as-user-rather-than-root

Short answer
I think what you want to do is:

Move or link the shell script somewhere that all users can access, and make sure each user has permission to execute the script.
For each user, create a user service (the kind that uses `systemctl --user`) that runs that script at boot time.
Each user service will be identical; the point is just that each user has their own service running for them at boot, linked to their `"${HOME}"/.config/systemd/user/` directory, and `systemctl --user enable`'d for them.

User services run as their user (not root), so each service will run as its corresponding user and inherit that user's "environment". That environment will be passed to any processes the service spawns -- namely, your shell script, where $HOME will expand to something like "/home/foo" when the foo user's service runs the script. When bar's service runs it, $HOME will expand to "/home/bar", and so on.

Further info
This is some general (and some niche) information which might help make some sense of what's going on.

First, there are system services and user services.

System services are the "default" kind of service, and the service you set up is probably one of these.

Their service files are linked in `/etc/systemd/system/`.
They run as root by default. ⚠️ (As a result, they have root permissions!)
Their spawned processes inherit NO environment (e.g., in a shell script run by the service, $HOME will actually be empty -- "").
They're controlled with `systemctl <command> <service>`.
User services are explicitly associated with the user who made them.

Their service files are linked in "${HOME}"/.config/systemd/user/ (you might have to create this directory if it doesn't exist).
They run as the user who made them by default (with that user's permissions, not root permissions).
Their spawned processes inherit that user's enviornment (e.g., in a shell script run by the service, $HOME will expand to /home/foo if a user named foo made the service).
They're controlled with systemctl --user <command> <service>
They also terminate when that user's session ends (this can be overriden by the enable-linger command you read about, but I don't think that's relevant to your problem here).
Ultimately, I want the service to recognize the current user and run the local script as the user with $HOME intact.

This sentence, in isolation, asks how to import the current user's environment (e.g., variables like $HOME) into a script that gets executed by a service.

There are many ways to import environment, from arguments to the commands that start the service to systemd settings to options set in the service files.

Here are 3 I know.

1. Simply re-declaring environmental variables
This is the most boneheaded way to do it.

If a user named foo wants $HOME to expand to "/home/foo" and $BARHOME to expand to "/home/bar", they can put this at the top of the script:

```
HOME=/home/foo
BARHOME=/home/bar
```
Easy! And a maintenance nightmare when a directory or file path changes and all of the scripts you did this with need to be updated to mirror the change.

2. Using the User= and Group= options in system services
I take it that this is what you referred to by "specifying a user in the service file" in:

I am wondering if there is a way to get the service to run as the current user (rather than specifying a user in the service file)

For system services, which run as root and have NO default environment, a foo user can put this section in the service's .service file:

```
[Service]
User=foo
Group=foo
```

This will cause the system service to run as foo (not root), with foo's permissions (no longer root's permissions), and with foo's environment. So, in a shell script executed by the service, $HOME will expand to "/home/foo", $USER will expand to "foo", and so on.

If you're wondering, using User=root and Group=root only has the effect of importing root's environment ($HOME will expand to "/root" instead of "", $USER will expand to "root", etc.); the system service still runs as root, with root permissions, as it does by default.

NOTE: These options are only allowed for system services. If a user service has these options in its [Service] section, it will error out when run. User services are all about being explicitly bound to a user, so they don't support running as a different user than the user who made and ran them.

The downside with importing a user's environment to a system service this way is that root permissions are lost, being replaced with that user's permissions. (If the service didn't need root permissions, no harm done, although it might as well be a user service in that case.)

Another downside is it doesn't at all assist with the "multi-user" aspect you're going for. (Again, making one user service for each user is probably the way to go for that.)

3. Using systemd's DefaultEnvironment variable
This is the way I import environmental variables. It might be a little weird, but it has two advantages:

It works independently of the User= and Group= options, decoupling a service's environment from its user.
It allows a user (e.g., root) to use their environmental variables simultaneously with a different user's (e.g., foo's) environmental variables.
We assume that the foo user declares all their environmental variables in their .profile (as is tradition). As an added twist, the foo user hardcodes their home directory in a variable named $UHOME. So, in /home/foo/.profile, we have:

```
export UHOME="/home/foo"
```

Similarly, $UBIN, where foo keeps binaries, can be defined in /home/foo/.profile like this:

```
export UBIN="${UHOME}"/bin
```
We'll see what this gets us in a second.

In /etc/systemd/system.conf, uncomment the DefaultEnvironment variable and, in it, declare an environmental variable named $UPROFILE hardcoded to reference foo's .profile:

DefaultEnvironment="UPROFILE=/home/foo/.profile"
Now, in any services' shell scripts, even system services', we can put this at the top of the script:

```
### Get foo's env vars.
source "${UPROFILE}"
```

Even when a system service, running as root, executes this script, foo's .profile will be sourced, and so foo's environmental variables will be loaded in, even alongside root's environmental variables (assuming the system service uses User=root and Group=root to inherit root's environment). So, in the script, $HOME will expand to root's home ("/root"), while $UHOME will expand to foo's home ("/home/foo"). That's what naming the variable $UHOME in foo's .profile bought us; it won't collide with root's $HOME environmental variable!

We don't have to worry about what user runs the service or what their environmental variables are! foo's environmental variables are always correct, so we always find foo's stuff. And, since foo's environmental variables are defined in one place (foo's .profile), changes to them there are automatically reflected in system scripts as they resource foo's .profile -- no script maintenance necessary.

This is nice for having your non-root user's environment in a system service script running as root.

However, this is completely unrelated to your question, which was:

I am wondering if there is a way to get the service to run as the current user (rather than specifying a user in the service file) so that $HOME is correct for the current user, allowing me to add as many users as I want, with all users having access to the script.

...If you can get away with this and don't mind how zany it is, you could have each user use their own environmental variable naming scheme in their .profiles, and environment inclusion method #3 here could be used to do whatever you want to do on boot for each user when just one user boots in.

E.g., user foo can define $FOOHOME in their .profile; user bar can define $BARHOME in their .profile; and so on.

In /etc/systemd/system.conf, you could reference each user's .profile:

DefaultEnvironment="FOOPROFILE=/home/foo/.profile" "BARPROFILE=/home/bar/.profile"
Then, at the top of the shell script, have:

```
### Get all users' env vars.
source "${FOOPROFILE}"
source "${BARPROFILE}"
```

Now, when a user boots in and the system service runs that script when a user boots in, $FOOHOME will expand to "/home/foo", $BARHOME will expand to "/home/bar", and so on.

BUT THAT'S JUST CRAZY FOOD FOR THOUGHT; what it sounds like you actually want is what I said in the Short answer section.

If you want a service to run when any user boots in and have $HOME expand to that user's home directory in the shell script run by the service, you don't have to mess with DefaultEnvironment. It's far simpler to create a user service for each user, and have the user services all run that script, which is in a location they can all access.
