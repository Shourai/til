# Enable system clipboard over ssh

Nvim bundles a clipboard provider that allows copying to the system clipboard
using OSC 52. OSC 52 is an Operating System Command control sequence that
writes the copied text to the terminal emulator. If the terminal emulator
supports OSC 52 then it will write the copied text into the system clipboard.
This is available from nvim version 0.10.0

The following setting is required for it to work:

```lua
vim.opt.clipboard = "unnamed,unnamedplus" -- allow neovim to access the system clipboard
vim.g.clipboard = {
 name = "OSC 52",
 copy = {
  ["+"] = require("vim.ui.clipboard.osc52").copy("+"),
  ["*"] = require("vim.ui.clipboard.osc52").copy("*"),
 },
 paste = {
  ["+"] = require("vim.ui.clipboard.osc52").paste("+"),
  ["*"] = require("vim.ui.clipboard.osc52").paste("*"),
 },
}

```

note: `unnamedplus` needs to be appended for it to work.

see also `:h clipboard-osc52`

## Testing OSC 52 compatability

Running the following command will copy "hello world" to the clipboard if the terminal emulator
supports OSC 52.

```
printf $'\e]52;c;%s\a' "$(base64 <<<'hello world')"
```

interesting discussions:

<https://github.com/LazyVim/LazyVim/discussions/4602>
