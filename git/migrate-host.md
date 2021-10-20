# Migrate git repo from one host to another

https://medium.com/collaborne-engineering/how-to-migrate-a-private-repository-from-bitbucket-to-github-6cddedd5d73

Step 1: Create Github repository
First, create a new private repository on Github.com. It’s important to keep the repository empty, e.g. don’t check option Initialize this repository with a README when creating the repository.

Step 2: Move existing content
Next, we need to fill the Github repository with the content from our Bitbucket repository:

Check out the existing repository from Bitbucket:
```
$ git clone https://USER@bitbucket.org/USER/PROJECT.git
```

2. Add the new Github repository as upstream remote of the repository checked out from Bitbucket:
```
$ cd PROJECT
$ git remote add upstream https://github.com:USER/PROJECT.git
```

3. Push all branches (below: just master) and tags to the Github repository:
```
$ git push upstream master
$ git push --tags upstream
```

Step 3: Clean up old repository
Finally, we need to ensure that developers don’t get confused by having two repositories for the same project. Here is how to delete the Bitbucket repository:
1. Double check that the Github repository has all content
2. Go to the web interface of the old Bitbucket repository
3. Select menu option Setting > Delete repository
4. Add the URL of the new Github repository as redirect URL

With that, the repository completely settled into its new home at Github. Let all the developers know!
That’s it.

