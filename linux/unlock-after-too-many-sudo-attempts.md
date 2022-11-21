# Unlock user after too many failed sudo attempts

https://joshtronic.com/2021/05/23/unlock-user-after-too-many-failed-sudo-attempts/


As many of you may already know, I tend to blog about the real-life problems that I’ve run into in the previous week or
so. This makes it easy to come up with blog content, but also gives me a dedicated resource to reference if/when one of
these problems pops up again.

My past week has been pretty tame in terms of running into problems, so when I sat down to write, I first had to come up
with a topic. While doing so, I was juggling updating my Arch Linux install on my personal laptop.

When I say juggling, I mean that I was bouncing around a few applications and I kept letting `sudo` time out on
accident. Having done this a few times, I eventually found myself locked out and unable to actually perform the
attempted upgrade because `sudo` was no longer accepting my password.

Sad truth is, I’ve had this happen before, I somehow fixed the situation. Because I hadn’t documented it with a blog
post, I had to go ahead and figure things out all over again.

Fortunately, it didn’t take much to get things working again. Also, the way that I got things resolved this time
actually seemed more streamlined than my previous attempt, which included me switching to a different TTY. If memory
serves it was such a process because I had logged out of GNOME and couldn’t log back in.

Not panicking this time, I stayed in GNOME and learned about my new best friend `faillock`.

The `faillock` command allows you to check for login failures and reset said records, allowing you to get back to
business.

Having never used this command before, I’m not entirely sure of it’s availability on other Linux distributions. I did
check my Debian install on my Intel NUC, and the command wasn’t available. Perhaps it needs to be explicitly installed
on Debian.

Anyway, it’s available on Arch Linux so for other distributions, YMMV.

What I found extremely peculiar, is that I was able to run `faillock` and unlock my current user AS my current user. I
had zero expectation for that to work and have no idea why a command for unlocking users after bad login attempts could
be run as the user in question and not require super user access.

So, to get back to being productive, I first ran `faillock` for my current user which revealed three failed login
attempts:

```
% faillock --user josh
josh:
When                Type  Source                                           Valid
2021-05-23 12:18:31 TTY   /dev/pts/7                                           V
2021-05-23 12:23:33 TTY   /dev/pts/7                                           V
2021-05-23 12:25:02 TTY   /dev/pts/7                                           V
```

Obviously, you should change `josh` to whatever user name you are attempting to unlock.

Having confirmed that my dumb ass did in fact lock myself out, I ran `faillock` again with the --reset argument:

```
% faillock --user josh --reset
```

Ding dong magic, things were back to normal and I was able to run yay -Syu and get my system up to date!
