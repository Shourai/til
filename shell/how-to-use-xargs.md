How to Use xargs on Linux
=========================

by [Shahriar Shovon](https://linuxhint.com/author/shahriar_shovon/)

**xargs** is a command line tool. If you want to redirect the output of a command as the argument of another command, then xargs is the tool for you. It is a very useful tool for easily doing a lot of stuff on the command line. In this article, I am going to show you how to use xargs on Linux. So, let's get started.

How xargs Works:
----------------

The format in which you use xargs command is:

$ command1 | xargs command2

You can also modify the behavior of xargs with some options. In that case, the format of the xargs command will be:

$ command1 | xargs [options] command2

Here, the output of the **command1** will be used as the argument of **command2**. The output of **command1** is broken down into many arguments by xargs depending on a character called delimiter. Then, xargs runs the command **command2** for each of these arguments and that argument is passed as the argument of the command **command2**.

For example, let's say, the output of **command1** is as follows:

value1 value2 value3

Let's say, the delimiter character is **space**. Now, the output of **command1** will be broken into 3 arguments, **value1**, **value2**, and **value3**.

Now, xargs runs the command **command2** for each of the 3 arguments once.

$ command2 value1\
$ command2 value2\
$ command2 value3

Here, **value1**, **value2**, and **value3** are the arguments parsed by xargs from the output of the command **command1**.

You can achieve the same effect using loops in a shell script. But xargs is just an easier way to do things without loops, especially on the command line.

By default, the delimiter of xargs is newline/space character. But you can change the delimiter character with the **-d** or **--delimiter** option of xargs.

By default, xargs works with one argument at a time. If you want to run the command **command2** with multiple arguments from the output of the command **command1**, then you can use the **-n** or **--max-args** option of xargs. Sometimes, you will have to tell xargs specifically to work with one argument at a time with the **-n** or **--max-args** option.

You can also append or prepend other strings to the arguments passed to the command **command2** using the **-I** option of xargs.

There are many other options of xargs, but these 3 are the most important and useful ones. So, I will only cover these 3 xargs arguments in this article.

That's enough blabbering. Let's go through some examples.

### Example 1: Creating and Removing Files Listed in a Text File

Let's say, you have a list of filenames in a text file **files.txt**.

You can see the contents of the text file **files.txt** as shown in the screenshot below.

$ cat files.txt

[![](https://linuxhint.com/wp-content/uploads/2020/02/1-4.png)](https://linuxhint.com/wp-content/uploads/2020/02/1-4.png)

Now, you can create all the files listed in the **files.txt** text file using the **touch** command with **xargs** as follows:

$ cat files.txt | xargs touch

[![](https://linuxhint.com/wp-content/uploads/2020/02/2-4.png)](https://linuxhint.com/wp-content/uploads/2020/02/2-4.png)

As you can see, the files are created as listed in the **files.txt**.

[![](https://linuxhint.com/wp-content/uploads/2020/02/3-4.png)](https://linuxhint.com/wp-content/uploads/2020/02/3-4.png)

Now, let's say, you want to remove the files which are listed in the **files.txt** text file. You can use the **rm** command with **xargs** as follows:

$ cat files.txt | xargs rm -v

[![](https://linuxhint.com/wp-content/uploads/2020/02/4-4.png)](https://linuxhint.com/wp-content/uploads/2020/02/4-4.png)

Only the files listed in the **files.txt** file are removed as you can see in the screenshot below.

[![](https://linuxhint.com/wp-content/uploads/2020/02/5-4.png)](https://linuxhint.com/wp-content/uploads/2020/02/5-4.png)

This is a very simple example of xargs.

### Example 2: Redirect STDOUT to Commands that Don't Support Pipe

You can redirect the STDOUT of a command **command1** as the STDIN of another command **command2** if command **command2** supports Linux pipe. But if the command does not support pipe, you won't be able to do that.

For example, the **echo** command does not support pipe. So, the following command won't print anything as you can see in the screenshot below.

$ date | echo

[![](https://linuxhint.com/wp-content/uploads/2020/02/6-4.png)](https://linuxhint.com/wp-content/uploads/2020/02/6-4.png)

xargs command can help you to redirect the STDOUT of **command1** (in this case **date**) to the STDIN of **command2** (in this case **echo**) as you can see in the screenshot below.

$ date | xargs echo

[![](https://linuxhint.com/wp-content/uploads/2020/02/7-4.png)](https://linuxhint.com/wp-content/uploads/2020/02/7-4.png)

### Example 3: Changing Delimiter of xargs

Here, I've printed a string **123-456-7890** (a dummy phone number) using xargs. As you can see, the whole output is treated as a single argument and xargs runs the **echo** command only once.

$ echo -n 123-456-7890 | xargs echo

[![](https://linuxhint.com/wp-content/uploads/2020/02/8-4.png)](https://linuxhint.com/wp-content/uploads/2020/02/8-4.png)

Here, I've changed the delimiter to **--** using the **-d** option of xargs. As you can see, the output **123-456-7890** is now treated as 3 different arguments **123**, **456**, and **7890**.

$ echo -n "123-456-789" | xargs -n 1 -d - echo

[![](https://linuxhint.com/wp-content/uploads/2020/02/9-4.png)](https://linuxhint.com/wp-content/uploads/2020/02/9-4.png)

### Example 4: Appending or Prepending xargs Arguments

You can append (add to the end of the argument) or prepend (add to the front of the argument) string to the argument passed to the command **command2** using xargs. Before I show you how to do this, I will show you how to use the **-I** option of xargs.

The **-I** option of xargs allows you to define a symbol for the xargs argument that is passed to the command **command2**. It works just like a variable.

For example,

$ echo -n "123-456-789" | xargs -d - -n 1 -I{} echo {}

Here, -I option defines **{}** as the symbol for the argument that xargs is currently working on. Once the symbol **{}** is defined, the symbol can be used to pass the argument to the command **command2**, which (the symbol **{}**) will be replaced by the value of the argument.

[![](https://linuxhint.com/wp-content/uploads/2020/02/10-4.png)](https://linuxhint.com/wp-content/uploads/2020/02/10-4.png)

Now, to append the string **.txt** (let's say) to each argument, you can use xargs as follows:

$ echo -n "123-456-789" | xargs -d - -n 1 -I{} echo {}.txt

[![](https://linuxhint.com/wp-content/uploads/2020/02/11-4.png)](https://linuxhint.com/wp-content/uploads/2020/02/11-4.png)

The same way, you can prepend the string **hello** (let's say) to each argument as follows:

$ echo -n "123-456-789" | xargs -d - -n 1 -I{} echo "hello {}"

[![](https://linuxhint.com/wp-content/uploads/2020/02/12-3.png)](https://linuxhint.com/wp-content/uploads/2020/02/12-3.png)

### Example 5: Changing Extensions of Specific Files

This one is a little bit tricky. But I will explain how it works. Don't worry.

Let's say, you have some files in your current working directory with different file extensions. Now, you want to change them all to **png** extension.

[![](https://linuxhint.com/wp-content/uploads/2020/02/13-2.png)](https://linuxhint.com/wp-content/uploads/2020/02/13-2.png)

You can change the file extension of all the files in your current working directory to png with xargs as follows:

$ ls | xargs -I{} bash -c 'FILE={} && mv -v $FILE ${FILE%%.*}.png'

[![](https://linuxhint.com/wp-content/uploads/2020/02/14.png)](https://linuxhint.com/wp-content/uploads/2020/02/14.png)

As you can see, all the file extension has changed to png.

[![](https://linuxhint.com/wp-content/uploads/2020/02/15.png)](https://linuxhint.com/wp-content/uploads/2020/02/15.png)

Here, xargs starts a bash sub shell and runs the bash command

FILE={} && mv -v $FILE ${FILE%%.*}.png

First, **FILE={}** assigns the symbol **{}** value, which is the filename (the argument value of xargs) to the **FILE** shell variable.

Then, **mv** command is used to change the file extension.

The **$FILE** shell variable contains the original filename.

**${FILE%%.*}** removes the extension of the filename (including . character) and then **.png** string is appended to the stripped filename.

xargs can do a lot more complex stuffs. Keep trying out new things with xargs. The sky is your limit.

If you need any help on xargs, you can check the man page of xargs as follows:

$ man xargs

[![](https://linuxhint.com/wp-content/uploads/2020/02/16-1024x490.png)](https://linuxhint.com/wp-content/uploads/2020/02/16.png)

So, that's how you use xargs on Linux. Thanks for reading this article.
