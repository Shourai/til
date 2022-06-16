# Sort lines based on a column in Vim/Nvim

https://jdhao.github.io/2021/12/21/sort_lines_based_on_a_column_nvim/


I have a file where each line has several comma-separated items. I want to sort the lines, based on a certain column. How do we do it inside Vim/Nvim?

There are two different ways to achieve this.
Using external sort

The first is via external tool sort and the vim filter feature (see :help filter). The syntax is:

```
:{range}!{filter}
```

Lines in the given {range} are sent to external program and are replaced with the output of external command.

For example, to sort the following lines based on the 3rd column:

```
adf,adf,0.56
dkkdf,iwer,0.23
uywe,qwe,0.4
mckdf,adfj,0.35
```

we can use the following vim command:

```
:%!sort -t, -k3
" or use the following
" :1,$!sort -t, -k3
```

Using :sort command

The second method is to use the vim builtin :sort command (see :h :sort for the details).

```
:sort f /\v^(.{-},){2}/
```

Explanation of the command:

    :sort: you can prefix it with a range, like 1,3sort. Without a range, the whole buffer is used (i.e., %sort).
    f: we are going to sort float numbers. We can also use x (hex number), b (binary number), o (octal number) etc.
    /\v^(.{-},){2}/: the pattern we are going to skip during sort, i.e., content after the pattern will be sorted.

The default is to sort the lines in ascending order. To sort the lines in descending order, add a ! after :sort (i.e., use :sort!).
References

    https://vi.stackexchange.com/a/5837/15292
    https://vi.stackexchange.com/a/7189/15292

