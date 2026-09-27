# Pipes: Data Processing Through Command Chaining

## Pipe (`|`)

Uses the **output (stdout) of one command as the input (stdin) of another**.

```bash
ls | wc -l     # count the entries in the current directory
```

**Counting only the errors** (combining pipes with redirection order):
```bash
du -h file1.txt file-not-exist.txt 2>&1 > /dev/null | wc -l
```
- `2>&1` sends stderr to where stdout currently goes → the pipe.
- `> /dev/null` then throws the normal output away.
- So only the error lines reach `wc -l`.

---

## Dual Output — The Utility `tee`

- With a combination of a pipe and `tee`, you can show standard output **and** write it to a file at the same time.

```bash
echo "Hello world" | tee hello.txt      # writes to hello.txt and also prints to stdout
echo "Hello world" | tee -a hello.txt   # -a → append instead of overwrite
ping google.com 2>&1 | tee ping.txt     # watch the output live and save it (errors too)
```

---

## `sort`, `uniq` (Commonly Used)

### `sort`
- Sorts the content of a file or stdin.
- Default: alphabetical order.

| Flag | Meaning |
|------|---------|
| `-r` | Reverse order |
| `-n` | Numerical sort |
| `-c` | Check whether content is sorted and report the first unsorted line |
| `-k [column_number]` | Sort data by a specific column (starting at 1) |
| `-u` | Sort and remove duplicate lines |

```bash
sort users.txt
sort -rn numbers.txt
sort -k 2 users.txt
```

### `uniq`
- Removes duplicate lines.
- ⚠️ Only removes **adjacent** duplicates — that's why it's almost always used after `sort`.

```bash
sort users.txt | uniq       # remove duplicates
sort -u users.txt           # another way — does the same thing
sort users.txt | uniq -d    # find duplicate entries (show only lines that appear more than once)
```

---

## Searching for Patterns — `grep` (Filter Lines)

`grep` finds a pattern in an output or a file, and prints only the matching lines.

```bash
grep -F 'pattern' file.txt       # search in a file
[command] | grep -F 'pattern'    # can also work on stdin
ls | grep -F 'file'              # takes any command that writes to stdout
```

**Notes:**
- By default `grep` uses **regex** (regular expressions).
- `-F` treats the pattern as a **fixed string** (plain text, no regex) — safer when the pattern contains characters like `.` or `*`.

**Example** — print out the IP address configuration:
```bash
ip addr show
ip addr show | grep -F 'inet' | grep -F '192.168'
```

### `grep` & Binary Data
- Can be used with text & binary files, but it's **not meant for binary** — use it for text only.
- `grep` with regex on binary files can be slow.
- Printing binary output may change the behavior of the terminal because of non-printable characters.

---

## Character Replacement & Reversal — `tr` & `rev`

### `tr` (translate) — for replacing characters

```bash
echo 'bash' | tr 'ba' 'db'             # b→d, a→b  → dbsh
```

- Works **character by character**, not with whole words.
- Also lets us work with character ranges:

```bash
echo 'awesome' | tr 'a-z' 'A-Z'        # lower → uppercase: AWESOME
echo 'AWESOME' | tr 'A-Z' 'a-z'        # upper → lowercase: awesome
```

**Delete characters** with `-d`:
```bash
echo 'bash is amazing' | tr -d ' '     # bashisamazing
```

### `rev` (reverse)
Reverses each line of text.

```bash
echo 'hello' | rev     # olleh
```

---

## Selective Extraction — The Program `cut`

- Processes / extracts data from a file or stdin.
- Can be used in different modes:

| Flag | Mode |
|------|------|
| `-b` | Cutting by **bytes** |
| `-c` | Cutting by **characters** |
| `-f` | Cutting by **fields** (used with `-d` for the delimiter) |

### By bytes
```bash
uptime | cut -b 1-10     # only interested in the first 10 bytes
```

### By characters
```bash
[command] | cut -c 2     # only the 2nd character
[command] | cut -c 2-    # from the 2nd character to the end
```

### By fields
```bash
cut -d ' ' -f 1 file.txt     # -d = delimiter (here a space), -f 1 = first field
uptime | cut -d ' ' -f 2     # second space-separated field
```

**Notes:**
- Every single delimiter counts — multiple spaces in a row create **empty fields**, so the field number might not be what you expect.

---

## `sed` (Stream Editor)

- Allows us to execute commands on stdin (or a file).

```bash
sed 'command1; command2; ...'
```

These commands can modify the text:
- Delete lines
- Modify lines
- Replace text (most common use of `sed`) → `s/[pattern]/[replacement]/[flags]`

```bash
echo 'hello world' | sed 's/world/home/g'   # hello home
echo 'hello world' | sed '/hello/d'         # deletes lines matching "hello"
```

- `g` flag = replace **all** occurrences in a line (without it, only the first one).
- Regex can also be used in the pattern.

**Notes:**
- `sed` might behave differently on different systems (e.g. GNU `sed` on Linux vs BSD `sed` on macOS — options like `-i` differ).
