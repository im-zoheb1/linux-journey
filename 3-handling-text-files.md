# Handling Text Files

## Viewing Text Content

### `cat`
Reads / shows the content of a file.

- We can also use globbing to show the content of multiple files.

```bash
cat some.txt
cat *.txt            # content of all .txt files
```

### `head` / `tail`

| Command | Meaning |
|---------|---------|
| `head -n 10 some.txt` | First 10 lines of the file |
| `tail -n 10 some.txt` | Last 10 lines of the file |

**Notes:**
- The default is **10** lines, so `-n 10` can be left out.

---

## Reading Large Files: `less`

```bash
less some.txt
less -N some.txt     # with line numbers
```

| Key | Meaning |
|-----|---------|
| `50p` | Jump to 50% of the content |
| `f` | Forward one page |
| `b` | Backward one page |
| `/pattern` | Search forward for a pattern |
| `?pattern` | Search backward for a pattern |
| `-N` | Toggle line numbers |
| `q` | Quit |

---

## Counting Words: `wc`

`wc` = **w**ord **c**ount.

```bash
wc romeo.txt         # lines, words, bytes
wc -lwc romeo.txt    # same, with explicit flags
```

| Flag | Meaning |
|------|---------|
| `-l` | Number of lines |
| `-w` | Number of words |
| `-c` | Number of bytes (= characters for 8-bit ASCII text) |

---

## Get Size of a File (Disk Usage): `du`

```bash
du                   # disk usage of the current directory (and subfolders)
du romeo.txt         # disk usage of a single file
du -sh .             # summary of the current directory, human readable
```

| Flag | Meaning |
|------|---------|
| `-s` | Summary (total only, no subfolder breakdown) |
| `-h` | Human readable (e.g. `4.0K`, `12M`) |
| `-k` | Sizes in kilobytes |

---

## Editing Files

| Editor | Notes |
|--------|-------|
| `pico` / `nano` | Simple, beginner-friendly editors |
| `vi` / `vim` | Powerful, but with a steeper learning curve |
