# How to split a subdirectory to a new git repo while keeping history

https://ao.ms/how-to-split-a-subdirectory-to-a-new-git-repository-and-keep-the-history/

Step 1: Clone your existing repo to a temp location

```
git clone https://github.com/ao/your_repo.git
cd your_repo
``````

Step 2: Checkout the branch where the subdirectory is

```
git checkout your_branch_name
```

Step 3: Run the Git Filter-Branch Command

The git filter-branch command allows you to prune empty entries and specify a subdirectory filter to base off:

```
git filter-branch --prune-empty --subdirectory-filter relative/path/to/subdirectory your_current_branch_name
```

Step 4: Update your new Git Remote

At this stage, you can go and create a new git repository, and copy the path:

```
git remote set-url origin https://github.com/ao/your_new_sub_repo.git
```

Step 5: Push your changes

You can now push your changes to your new repository:

```
git push -u origin your_current_branch_name
```
