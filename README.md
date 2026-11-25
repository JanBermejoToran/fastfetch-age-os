# fastfetch-age-os

A custom [fastfetch](https://github.com/fastfetch-cli/fastfetch) module that displays how long your Linux system has been installed.

```
Age OS : 128 days
```

## Overview

`age_os` is a single bash script with no dependencies. It reads the birth time of the root filesystem (`stat -c %W /`), compares it with the current time and prints one of:

| Output    | Condition                                          |
| --------- | -------------------------------------------------- |
| `Today`   | Less than 24 hours since installation              |
| `1 day`   | Exactly one day since installation                 |
| `N days`  | Any other number of days                           |
| `Unknown` | Filesystem does not expose birth time (exit code 1) |

## Requirements

- Linux with bash
- [fastfetch](https://github.com/fastfetch-cli/fastfetch)
- `curl`
- A root filesystem that supports birth time (ext4, btrfs, xfs, among others)

## Installation

### 1. Download the script

```bash
mkdir -p ~/.local/bin
curl -fsSL https://raw.githubusercontent.com/JanBermejoToran/fastfetch-age-os/main/age_os -o ~/.local/bin/age_os
chmod +x ~/.local/bin/age_os
```

### 2. Verify it works

```bash
~/.local/bin/age_os
```

Expected output: a value such as `128 days`.

### 3. Add the module to your fastfetch config

Open your fastfetch configuration file (default: `~/.config/fastfetch/config.jsonc`). If you do not have one, generate it:

```bash
fastfetch --gen-config
```

Add the following object to the `modules` array, in the position where you want it displayed:

```jsonc
{
  "type": "command",
  "key": "Age OS",
  "text": "~/.local/bin/age_os"
}
```

Example:

```jsonc
"modules": [
  "title",
  "separator",
  "os",
  "host",
  "kernel",
  {
    "type": "command",
    "key": "Age OS",
    "text": "~/.local/bin/age_os"
  },
  "uptime",
  "packages"
]
```

### 4. Run fastfetch

```bash
fastfetch
```

## Customization

| Goal                    | How                                         |
| ----------------------- | ------------------------------------------- |
| Change the label        | Edit `key` in the module (e.g. `"Installed"`) |
| Use another script path | Edit `text` in the module                   |
| Change the output text  | Edit the strings in `age_os`                |

## Troubleshooting

- **The `Age OS` line is empty:** the script is not at `~/.local/bin/age_os` or is not executable. Run `chmod +x ~/.local/bin/age_os`.
- **The line shows `Unknown`:** the root filesystem does not store birth time. Check with `stat -c %W /`; a value of `0` or `-1` means it is unavailable.
- **fastfetch reports a config error:** check for missing or extra commas around the added module.
- **The config is not loaded:** run `fastfetch --config ~/.config/fastfetch/config.jsonc` to confirm the path.

## Uninstall

Remove the module object from your fastfetch config, then delete the script:

```bash
rm ~/.local/bin/age_os
```

## License

MIT
