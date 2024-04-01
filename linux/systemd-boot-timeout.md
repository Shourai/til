# Change systemd-boot timeout

<https://wiki.archlinux.org/title/systemd-boot>

The loader configuration is stored in the file `*esp*/loader/loader.conf`. See [loader.conf(5) § OPTIONS](https://man.archlinux.org/man/loader.conf.5#OPTIONS) for details.

A loader configuration example is provided below:

*esp*/loader/loader.conf

default  arch.conf
timeout  4
console-mode max
editor   no

**Tip:**

- systemd-boot does not accept tabs for indentation, use spaces instead.
- `default` and `timeout` can be changed in the boot menu itself and changes will be stored as UEFI variables `LoaderEntryDefault` and `LoaderConfigTimeout`, overriding these options.
- `bootctl set-default ""` and `bootctl set-timeout ""` can be used to clear the UEFI variables overriding the `default` and `timeout` options, respectively.
- If you have set `timeout 0`, the boot menu can be accessed by pressing `Space`.
- A basic loader configuration file is located at `/usr/share/systemd/bootctl/loader.conf`.
- If the bootloader (during the entry selection) appears distorted/uses the wrong resolution you can try to set the `console-mode` to `auto` (uses heuristics to select the best resolution), `keep` (keeps the firmware provided resolution) or `2` (tries to select the first non-UEFI-standard resolution).
