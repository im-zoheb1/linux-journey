# Redirection: Managing Data Streams

## Writing and Appending Output — Redirection Operators `>` & `>>`

| Operator | Meaning |
|----------|---------|
| `>` | Writes output to a file. If the file exists it is **overwritten**, otherwise it is created. |
| `>>` | **Appends** output to the end of a file (creates it if it doesn't exist). |

```bash
echo "first line" > notes.txt     # create / overwrite
echo "second line" >> notes.txt   # append
```

**Why can some output be redirected and some can't?**
- `>` and `>>` only redirect **stdout**. Error messages go to **stderr**, so they still show up in the terminal (see below).

---

## Standard Streams — `stdin`, `stdout`, `stderr`

Every command has 3 communication channels by default:

| # | Stream | Default |
|---|--------|---------|
| `0` | Standard input — `stdin` | From the keyboard |
| `1` | Standard output — `stdout` | On screen |
| `2` | Standard error — `stderr` | On screen |

- `>` / `>>` redirect **stdout** to a file — it's no longer sent to the terminal, but to the file.

**Example** — let's say `2.jpg` doesn't exist:

```bash
du -h 1.jpg 2.jpg > out.txt
```
- The size of `1.jpg` is captured in `out.txt`, but the error for `2.jpg` is **not** — it's still printed in the terminal (it goes to stderr).

---

## Managing Error Messages: Redirecting `stderr` (& `stdout`)

The more verbose way of writing a stdout redirect — `1` stands for stdout:

```bash
du -h some.jpg 1> output.txt      # same as: du -h some.jpg > output.txt
```

**Redirect stderr (`2`):**
```bash
du -h some.jpg undef.jpg > output.txt 2> errors.txt
```
- Sizes go to `output.txt`, errors go to `errors.txt`.

### Ignoring Errors

Sometimes we want to ignore error messages. `/dev/null` is a special file that throws away everything written to it.

```bash
du -h some.jpg some2.jpg 2> /dev/null   # hide errors, show only output
```

**If we only care about errors:**
```bash
du -h some.jpg some2.jpg > /dev/null    # stdout goes to /dev/null, only errors are shown
```

---

## Redirect `stderr` to `stdout`

**Why?** It allows both outputs to be stored in the same file.

**① By repeating the file name**
```bash
du -h some1.jpg some2.jpg 1> out.txt 2> out.txt
```
- ⚠️ Not reliable: the file is opened twice separately, so the two streams can **overwrite each other**. Prefer ③.

**② Later: piping** to chain commands together
- A pipe (`|`) only passes **stdout** to the next command — errors aren't passed along unless we redirect them to stdout first.

**③ `&1` — stands for "the current stdout"**
```bash
[command] 2>&1                  # send stderr wherever stdout goes
[command] > out.txt 2>&1        # both stdout and stderr into out.txt
```

```bash
du -h some.txt undef.txt 2>&1              # both shown in terminal
du -h some.txt undef.txt > out.txt 2>&1    # both written to out.txt
```

**Why is the order of lines different in the output file than in the terminal?**
- Because **stdout is buffered** — it waits a little bit to collect more data before writing (there could be a large amount of data).
- stderr is **not** buffered, so the error gets written first.
- In the terminal, stdout is flushed after every line, so the order looks "normal".

> Bash shorthand: `&> out.txt` is the same as `> out.txt 2>&1`.

---

## Redirect `stderr` — Why Order Is So Important

Let's compare:

```bash
[command] > out.txt 2>&1    # 1
[command] 2>&1 > out.txt    # 2
```

**1 works fine** — both end up in `out.txt`:
1. stdout is redirected to `out.txt`
2. stderr is redirected to the same destination as stdout → `out.txt`

**2 doesn't do what we want** — stderr is printed in the terminal, only stdout goes to the file:
1. stderr is redirected to the same destination as stdout → at this point, that's still the **terminal**
2. then stdout is redirected to `out.txt`

**Why?** Redirections are processed **left to right**, so order is extremely important.

---

## Redirecting `stdin`

```bash
wc -l          # no file given → waits for input from the keyboard (end with Ctrl+D)
```

- Instead of typing, another command's output or a file can be used as input.
- `<` is used for **input** redirection:

```bash
wc -l < out.txt                  # read stdin from out.txt
cat - < out.txt > another.txt    # `-` means "read from stdin"; copies out.txt into another.txt
```
