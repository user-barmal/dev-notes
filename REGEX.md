# Bash
Used in grep.

# Python

# Notepad++

Groups:

Definition: positional () () ()
Call: (\1) (\2) (\3)

```text
Search for: "something (this will be a group)"
Replacement string: "sth different (\1)"
Output: "sth different this will be a group"
```

| Action | grep |
|--------|------|
| or     | `a&#124;b`  |
| -      | `a*`   |
| -      | `a\+`  |
| -      | `a\?`  |
| -      | `a\{n\}` |
| -      | `a\{n,k\}` |
| -      | `\(abc\)` |
| -      | `[abc]`   |
| -      | `^line` |
| -      | `line$` |
| ignore case | -i/--ignore-case |
