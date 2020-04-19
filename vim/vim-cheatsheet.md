
# Vim Cheatsheet

## Cursor Movement

    h                   move left
    j                   move down
    k                   move up
    l                   move right
    w                   jump by start of words (punctuation considered words)
    W                   jump by words (spaces separate words)
    e                   jump to end of words (punctuation considered words)
    E                   jump to end of words (no punctuation)
    b                   jump backward by words (punctuation considered words)
    B                   jump backward by words (no punctuation)
    0                   jump to start of line (zero)
    ^                   jump to first non-blank character of line
    $                   jump to end of line
    -                   move line upwards, on the first non blank character
    +                   move line downwards, on the first non blank character
    <enter>             move line downwards, on the first non blank character
    gg                  go to first line
    G                   go to last line
    nG                  go to line n
    :n                  go to line n
    10%                 move to 10% of the file
    15|                 move to the 15th column
    )                   move the cursor forward to the next sentence.
    (                   move the cursor backward by a sentence.
    {                   move the cursor a paragraph backwards
    }                   move the cursor a paragraph forwards
    ]]                  move the cursor a section forwards or to the next {
    [[                  move the cursor a section backwards or the previous {
    CTRL-f              move the cursor forward by a screen of text
    CTRL-b              move the cursor backward by a screen of text
    CTRL-u              move the cursor up by half a screen
    CTRL-d              move the cursor down by half a screen
    H                   move the cursor to the top of the screen.
    M                   move the cursor to the middle of the screen.
    L                   move the cursor to the bottom of the screen.
    fx                  search line forward for 'x'
    Fx                  search line backward for 'x'
    tx                  search line forward before 'x'
    Tx                  search line backward before 'x'
    ;                   go to next f/t search result
    ,                   go to previous f/t search result


## Bookmarks

    :marks              list all the current marks
    ma                  make a bookmark named a at the current cursor position
    `a                  go to position of bookmark a
    'a                  go to the line with bookmark a
    `.                  go to the line that you last edited
    ['                  jump to previous bookmark
    ]'                  jump to the next bookmark
    '<                  jump to the beginning of the last visible mode selection area
    '>                  jump to the end of the last visible mode selection area


## Insert Mode

    i                   enter insert mode at cursor
    I                   enter insert mode at the beginning of the line
    a                   enter insert mode after the cursor
    A                   enter insert mode at the end of the line
    o                   insert a new line below current line and enter insert mode
    O                   insert a new line above current line and enter insert mode
    gi                  enter inset mode at last insert.
    <ESC> / CTRL-[      exit insert mode

    CTRL-W              delete words backwards
    CTRL-O              temporarily exits insert mode, executes a single command and returns to insert mode
    CTRL-\ CTRL-O       Temporarily exits insert mode (cursor hold), executes a single command and returns to insert mode
    CTRL-R 0            inserts the contents of the register (internal 0 clipboard), and can be followed by the register name after CTRL-R
    CTRL-R "            Insert anonymous register contents, equivalent to p-paste in insert mode
    CTRL-R =            insert expression evaluates result, equal sign followed by expression
    CTRL-R :            Insert last command line command
    CTRL-R /            insert the keyword of the last search
    CTRL-F              auto indent
    CTRL-U              deletes all characters in the current line
    CTRL-V {char}       inserts a non-numeric literal
    CTRL-V {number}     inserts ascii/unicode characters represented by three numbers
    CTRL-V 065          inserts decimal ascii characters (two numbers) 065 ie A characters
    CTRL-V x41          insert hexadecimal ascii character (three digits) x41 ie A character
    CTRL-V o101         Insert octal ascii character (three digits) o101 ie A character
    CTRL-V u1234        insert hexadecimal unicode characters (four digits)
    CTRL-V U12345678    Insert hexadecimal unicode characters (eight digits)
    CTRL-K {ch1} {ch2}  Insert digraph (see: h digraph), quickly input Japanese or symbols, etc.


## Editing

    r                   replace a single character (does not use insert mode)
    R                   enter Insert mode, replacing characters rather than inserting
    J                   join line below to the current one
    cc                  change (replace) an entire line
    cw                  change (replace) to the end of word
    c$                  change (replace) to the end of line
    s                   delete character at cursor and substitute text
    S                   delete line at cursor and substitute text (same as cc)
    xp                  transpose two letters (delete and paste, technically)
    u                   undo
    CTRL-r              redo
    .                   repeat last command
    ~                   switch case
    g~iw                switch case of current word
    gUiw                make current word uppercase
    guiw                make current word lowercase
    gU$                 make uppercase until end of line
    gu$                 make lowercase until end of line
    >>                  indent line one column to right
    <<                  indent line one column to left
    ==                  auto-indent current line
    ddp                 swap current line with next
    ddkp                swap current line with previous
    :%retab             fix spaces / tabs issues in whole file
    :r [name]           insert the file [name] below the cursor.
    :r !{cmd}           execute {cmd} and insert its standard output below the cursor.
    CTRL-a              increment number
    CTRL-x              decrement number


## Deleting text

    x                   delete current character
    X                   delete previous character
    dw                  delete the current word
    dd                  delete (cut) a line
    D                   delete from cursor to end of line
    :[range]d           delete [range] lines


## Copying and moving text

    yiw                 yank word
    yy                  yank (copy) a line
    2yy                 yank 2 lines
    y$                  yank to end of line
    p                   put (paste) the clipboard after cursor/current line
    P                   put (paste) before cursor/current line
    v0                  select from cursor to beginning of line
    v$                  select from cursor to beginning of line
    viw                 select word
    gv                  reselect last selection

    :set paste          avoid unexpected effects in pasting

    :registers          display the contents of all registers
    "xyw                yank word into register x
    "xyy                yank line into register x
    :[range]y x         yank [range] lines into register x
    "xp                 put the text from register x after the cursor
    "xP                 put the text from register x before the cursor
    "xgp                just like "p", but leave the cursor just after the new text
    "xgP                just like "P", but leave the cursor just after the new text
    :[line]put x        put the text from register x after [line]


## Macros

    qa                  start recording macro 'a'
    q                   end recording macro
    @a                  replay macro 'a'
    @:                  replay last command
    @@                  replay last macro


## Visual mode

    v                   start visual mode, mark lines, then do command (such as y-yank)
    V                   start linewise visual mode
    o                   move to other end of marked area
    U                   upper case of marked area
    CTRL-v              start visual block mode
    O                   move to other corner of block
    aw                  mark a word
    ab                  a () block (with braces)
    ab                  a {} block (with brackets)
    ib                  inner () block
    ib                  inner {} block
    Esc                 exit visual mode

## Visual mode commands

    >                   increase indentation
    <                   decrease indentation
    c                   change (replace) marked text
    y                   yank (copy) marked text
    d                   delete marked text
    x                   delete marked text
    ~                   switch case
    o                   jump to the other end of highlighted text
    O                   jump to the other end of highlighted line(s)
    u                   highlighted area converted to lowercase
    U                   highlighted area converted to uppercase
    g CTRL-g            display statistics of highlighted area


## Spelling

    :set spell          enable spellchecking
    :set nospell        disable spellchecking
    ]s                  next misspelled word
    [s                  previous misspelled word
    zg                  add word to wordlist
    zug                 undo last add word
    z=                  suggest word


## Exiting

    :q                  quit Vim. This fails when changes have been made.
    :q!                 quit without writing.
    :cq                 quit always, without writing.
    :wq                 write the current file and exit.
    :wq!                write the current file and exit always.
    :wq {file}          write to {file}. Exit if not editing the last
    :wq! {file}         write to {file} and exit always.
    :[range]wq[!]       same as above, but only write the lines in [range].
    ZZ                  write current file, if modified, and exit.
    ZQ                  quit current file and exit (same as ":q!").


## Search / Replace

    /pattern            search for pattern
    ?pattern            search backward for pattern
    n                   repeat search in same direction
    N                   repeat search in opposite direction
    *                   Search forward, word under cursor
    #                   Search backward, word under cursor
    :s/old/new/g        replace all old with new on current line
    :%s/old/new/g       replace all old with new throughout file
    :%s/old/new/gc      replace all old with new throughout file with confirmations
    :10,20s/old/new/g   replace all old with new in lines 10 to 20


## Multiple files

    :e filename         edit a file in a new buffer
    :tabe filename      edit a file in a new tab (Vim7, gVim)
    :ls                 list all buffers
    :bn                 go to next buffer
    :bp                 go to previous buffer
    :bd                 delete a buffer (close a file)
    :b1                 show buffer 1
    :b vimrc            show buffer whose filename begins with "vimrc"


## Windows

    :sp <filename>      split open <filename>
    :vsp <filename>     vsplit open <filename>
    CTRL-w s            horizontally split windows
    CTRL-w v            vertically split windows
    CTRL-w w            jump to next window
    CTRL-w W            jump to previous window
    CTRL-w p            jump to last visited window
    CTRL-w q            quit a window
    CTRL-w x            swap windows
    CTRL-w h            left window
    CTRL-w j            down window
    CTRL-w k            up window
    CTRL-w l            right window
    CTRL-w +            increase lineheight of current window
    CTRL-w -            reduce lineheight of current window
    CTRL-w >            increase column width of current window
    CTRL-w <            reduce column width of current window
    CTRL-w =            make all windows the same width and height
    CTRL-w o            close other windows
    CTRL-w c            close current window
    CTRL-w H            moves the current window to the far left
    CTRL-w J            moves the current window to the bottom
    CTRL-w K            moves the current window to the top
    CTRL-w L            moves the current window to the far right
    CTRL-w x            swap window
    CTRL-w f            opens a file named file name under the cursor in a new window
    CTRL-w gf           opens a file named file name under the cursor in a new tab
    CTRL-w R            rotate window
    CTRL-w T            move the current window to a new tab
    CTRL-w P            jumps to the preview window
    CTRL-w z            closes the preview window
    CTRL-w _            maximize current window horizontally
    CTRL-w |            maximize current window vertically

## Programming

    %                   show matching brace, bracket, or parenthese
    gf                  edit the file whose name is under or after the cursor
    gd                  when the cursor is on a local variable or function, jump to its declaration
    ''                  return to the line where the cursor was before the latest jump
    gi                  return to insert mode where you inserted text the last time
    CTRL-o              move to previous position you were at
    CTRL-i              move to more recent position you were at

## External commands
    :!ls                runs the external command ls and waits for a return
    :r !ls              captures the output of the external command ls and inserts it into the cursor
    :w !sudo tee %      sudo save the current file later
    :call system('ls')  calls the ls command, but does not display the returned content
    :!start notepad     start notepad under Windows, you can add silent at the front
    :sil !start cmd     open cmd in current directory under Windows
    :%!prog             run text filter, such as json format: %!python -m json.tool


## Settings

    :set ignorecase     sets whether the search ignores case
    :set spell          enable spell check
    :set nospell        disable spell check
    :set hlsearch       enable search highlighting
    :syntax on          enable syntax highlighting
    :syntax off         disable syntax highlighting
    :set nowrap         disable linewrapping
    :set number         enable line numbering
    :set relativenumber enable relative line numbering
    :set all            list all options
    :set nohl           clear search highlighting

## Miscellaneous
    CTRL-X CTRL-F       file path completion in insert mode
    CTRL-X CTRL-O       inserts under Omnifunc completion
    CTRL-X CTRL-N       keyword completion in insert mode
    CTRL-X CTRL-E       scrolls up in insert mode
    CTRL-X CTRL-Y       scroll down in insert mode
    CTRL-E              scrolls up
    CTRL-Y              scrolls down
    CTRL-G              shows the name of the file being edited, as well as the size and location information
    g CTRL-G            shows the size of the file: size, number of characters, number of words and number of lines, also available in visual mode
    Zz                  adjust the line of the cursor to the center of the screen
    Zt                  adjust the line where the cursor is located to the top of the screen
    Zb                  adjust the line where the cursor is to the bottom of the screen
    Ga                  displays the ascii code or unicode code of the character under the cursor
    G8                  shows the UTF-8 encoded endian of the character under the cursor
    Gi                  back to the last place to insert and switch to insert mode
    K                   query the help of the word under the cursor
    ZZ                  save the file (if any) and close window
    ZQ                  does not save the file and close window
    :.!date             insert time and date in the current window

## Vimdiff
    ]c                  next difference
    [c                  previous difference
    do                  diff obtain
    dp                  diff put
    zo                  open folded text
    zc                  close folded text
    :diffupdate         re-scan the files for differences

## References
https://github.com/skywind3000/awesome-cheatsheets/blob/master/editors/vim.txt \
https://blog.g-design.net/post/4789778607/vim-cheat-sheet \
https://github.com/hackjutsu/vim-cheatsheet \
http://www.fprintf.net/vimCheatSheet.html \
http://michael.peopleofhonoronly.com/vim/ \
https://github.com/glts/vim-cottidie/blob/master/autoload/cottidie/tips \
https://gist.github.com/azadkuh/5d223d46a8c269dadfe4 \
https://devhints.io/vim