# Globbing & File Searching

## Bash on macOS

- Pre-installed and open source.
- `zsh` is the default shell on macOS.
- macOS ships **Bash version 3** by default, located at `/bin/bash`.
- For Bash 5.x on macOS, install it with Homebrew:

```bash
brew update
brew install bash
```

---

## Filename Expansion (Globbing)

- Bash can rewrite our commands before they're executed.
- Makes it easy to access multiple files at once.
- e.g. a shorthand to move multiple images to a folder — use globbing for this.

### Wildcard Characters

**`*`** — matches any number of characters

```bash
mv *.jpg images/     # move all .jpg files into images/
mv images/* .        # move everything out of images/ into the current directory
```

- `*` does **not** include hidden files (starting with `.`). For those, use `.*`:

```bash
mv images/.* .       # move hidden files out of images/
```

**Notes:**
- Our commands don't know about globbing — **Bash translates the pattern** into the list of matching files before running the command.
- Wildcard characters **must not be quoted** (`"*.jpg"` is taken literally).
- Globbing does **not** use regular expressions.

---

## Advanced Globbing Wildcards — `?`, `[0-9]`, `**`

| Wildcard | Meaning |
|----------|---------|
| `?` | Matches any single character |
| `[0-9]` | Matches one character from a range (e.g. `[a-z]`, `[0-9]`) |
| `**` | Matches zero or more directories (including `/`) — recursive |

### `?`
```bash
echo IMG?9337.jpg    # matches IMG_9337.jpg
```

### `[0-9]`
```bash
echo ./images/IMG_6[0-9][0-9].[a-z][a-z][a-z]   # e.g. IMG_612.jpg
```

### `**`
- Might need to be enabled first: `shopt -s globstar`
- Requires **Bash 4.0+** (not available in macOS's default Bash 3).

```bash
shopt -s globstar
echo **/*.jpg        # all .jpg files in this folder and every subfolder
```

---

## Pitfalls of Globbing (Avoiding Patterns)

`rm -rf` → `-r` means **recursive**, `-f` means **don't ask** (force).

**Creating a file named `-rf`:**
- `touch '-rf'` does **not** work — quotes have no special meaning here, Bash removes them and `touch` still sees `-rf` as an option.
- Instead, use:

```bash
touch ./-rf
```

**Why this is dangerous:**
- If a file called `-rf` exists, `rm *` expands to something like `rm -rf file1 folder1 ...`.
- `rm` treats `-rf` as a **parameter**, not a filename — so instead of deleting just files, it deletes **all files and folders** recursively, without asking (and leaves the `-rf` file itself).

**How to protect ourselves:** whenever using `*`, prefix it with `./`

```bash
rm ./*       # expands to ./-rf ./file1 ... — now this works properly
```

---

## Misc

- `tree` — shows a directory tree. May or may not be available (might need to install it).

**Combining globs with brace expansion:**
```bash
cp */0[1-2]*/*.{pdf,xlsx} Export
```
Copies all `.pdf` and `.xlsx` files from subfolders whose names start with `01` or `02` into `Export/`.
- Note: no spaces inside `{pdf,xlsx}`.

---

## Sophisticated File Searching (`find`)

| Command | Meaning |
|---------|---------|
| `find . -type d` | Directories only |
| `find . -type f` | Files only |
| `find . -type f -mtime -7` | Files modified in the last 7 days (`-mtime` = modification time) |
| `find . -type f -size +1M` | Files larger than 1 MB |
| `find . -type f -size +1M -delete` | Delete files larger than 1 MB |
| `find . -empty -delete` | Delete empty files and folders |

**Notes:**
- `-delete` removes everything matched — run the command without `-delete` first to check what it will delete.

```bash
find . -type d
find . -type f -mtime -7
find . -type f -size +1M
find . -empty -delete
```
