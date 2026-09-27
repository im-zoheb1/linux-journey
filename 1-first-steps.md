# Linux Terminal Basics

## `echo`
Outputs text to the terminal.

| Flag | Meaning |
|------|---------|
| `-n` | No trailing line break |
| `-e` | Enable backslash escapes (e.g. `\n`, `\t`) |
| `-ne` / `-en` | Both combined — order doesn't matter |

**Notes:**
- Quotes in `echo` are *not* like quotes in a programming language — they're just used to group text, not to define a string type.

```bash
echo "Hello, world"
echo -n "No newline after this"
echo -e "Line1\nLine2"
echo -ne "No newline\twith a tab"
```

---

## `pwd` / `cd`
`pwd` = **p**rint **w**orking **d**irectory.

- Linux has no drive-letter system (like `C:\` on Windows) — everything lives under a single root folder tree.
- `/` refers to the **root** directory (top of the filesystem).
- `~` refers to the current user's **home** directory.
- Some shells are configured to show `~` or `$` as part of the prompt to indicate the current location.

**Navigating with `cd`:**
```bash
cd ~          # go to home directory
cd /          # go to root directory
cd ..         # move up one level
cd ../..      # move up two levels
cd -          # go to the previous directory
```

---

## `ls` — list directory contents

**Syntax:**
```bash
ls [options] [path]
```

| Flag | Meaning |
|------|---------|
| `-a` | Show hidden files (those starting with `.`) |
| `-r` | Reverse the sort order |
| `-t` | Sort by modification time (newest first) |
| `-tr` | Sort by time, reversed (oldest first) |
| `--color` | Enable colorized output |
| `--color=never` | Disable colors |
| `--color={always,never,auto}` | Explicitly set color mode |

**Notes:**
- Single-dash flags can be combined (e.g. `-tr`, `-la`), but **long (`--`) options cannot be combined** with each other or with short flags in that shorthand way.

```bash
ls -la              # long format, including hidden files
ls -ltr              # sorted by time, oldest first, long format
ls --color=auto      # color only when output is a terminal
```

## Absolute vs Relative Paths

**Absolute**
- starts with `/` or `~`

**Relative**
- relative to the current working directory

```bash
./{name}/
```

## `;` — Executing Multiple Commands

```bash
cd .. ; cd Desktop ; ls
```

## Help

| Flag/Command | Meaning |
|------|---------|
| `-h` / `--help` | Show help for a command |
| `ls -h` | Example: help for `ls` |
| `man` | Manual |
| `man ls` | Manual page for the `ls` command |

**Notes:**
- `man` is not available sometimes, so we have to unminimize it.

**`sudo unminimize`**
- `sudo apt install man-db` (downloads man pages)
- `sudo reboot`

---

## User Management Basics

There are 3 categories of users:

| Type | Description |
|------|-------------|
| **System accounts** | Run background tasks/services. Usually have no home directory. |
| **Regular users** | Access to their own files. Cannot perform admin tasks. |
| **Super / root user** | Can modify system configuration and other users. |

### `sudo`
- Gives a regular user temporary additional (root) privileges for a single command.
- Standard users can't use `sudo` unless they're explicitly allowed to.
- **Always be careful with `sudo`** — it bypasses the usual safety checks.

```bash
sudo rm -rf /etc     # DON'T RUN THIS — will destroy the system
```

---

## Package Management

- A way to install/update software.
- Slightly different for each distro, but the idea remains the same.
- On **Ubuntu**, the tool is `apt`, which provides several commands.

### Updating / Installing Software

| Command | Meaning |
|---------|---------|
| `apt update` | Refreshes the list of available packages — run this before anything else |
| `apt upgrade` | Upgrades existing packages |
| `apt full-upgrade` / `apt dist-upgrade` | Larger upgrade — updates **and can remove** existing packages to resolve dependencies |
| `apt remove [pkg name]` | Removes a package |
| `apt autoremove` | Removes packages that are no longer being used |

**Notes:**
- `full-upgrade` can break things sometimes.
- `autoremove` can occasionally cause issues too — check what it will remove first.
- `apt-get` vs `apt`: `apt-get upgrade` only updates existing packages, while `apt upgrade` also installs new dependencies if needed. `apt` is the newer, friendlier interface.

```bash
sudo apt update
sudo apt upgrade
sudo apt install man-db
sudo apt remove man-db
sudo apt autoremove
```
