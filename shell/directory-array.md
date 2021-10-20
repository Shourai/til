# Create and populate an array of directories in bash

```
#!/bin/bash
arr=(/my/directory/path/*/)    # This creates an array of the full paths to all subdirs
arr=("${arr[@]%/}")            # This removes the trailing slash on each item
arr=("${arr[@]##*/}")          # This removes the path prefix, leaving just the dir names
```
