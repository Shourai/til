# Debug option settings

For many Vim customizations (eg tabstop setting, BufEnter autocmd) 
you can do 

```
:verbose [option]? 
```

(eg `:verbose set tabstop?` or `:verbose autocmd BufEnter`)
to find the current values and where they were set.
Modelines and scripts which run after .vimrc can trip you up in this way. 
