# Environment Variables: Managing Shell Configuration

## Shell Environment

A collection of settings, variables, aliases and configuration that the shell uses.

## What Is a Shell?

- The outer layer of the OS.
- Takes commands and translates them for the OS.

**In general:** a shell is everything that allows the outside world to access the OS — so even a **GUI** is considered a shell.

When we say "shell" here, we're referring to the **command line / terminal / console**.

---

## How to Access Environment Variables

- Convention: environment variables are written in **UPPERCASE** (`HOME`, `PATH`, `USER`).
- Bash variables work slightly differently from environment variables — by convention they're written in **lowercase** (so they don't clash with environment variables).

### `env`
Lists all environment variables.

```bash
env
```

### Access a single variable

```bash
echo "${PWD}"
echo "${USER}"
echo "${PATH}"
```

We can also use:
```bash
echo "$PATH"     # braces are optional when nothing follows the name
```

Also possible, but **avoid**:
```bash
echo $PATH       # unquoted — the value can get split on spaces or expanded as a glob
```

**Notes:**
- Braces are needed when text directly follows the variable, e.g. `"${USER}_backup"`.

---

## Common Environment Variables

| Variable | Meaning |
|----------|---------|
| `HOME` | Stores the user's home directory path |
| `PWD` | Stores the current working directory path |
| `OLDPWD` | Stores the previous working directory path (used by `cd -`) |
| `USER` | The current user's name |
| `PATH` | List of directories (separated by `:`) where the shell looks for commands |

```bash
echo "${HOME}"
echo "${PWD}"
echo "${OLDPWD}"
```
