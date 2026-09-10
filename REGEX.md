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

# Comparison Table

| Action      | grep (BRE)        | grep -E (ERE)     |
|-------------|-------------------|-------------------|
| or          | `a\|b`            | `a\|b`            |
| 0 or more   | `a*`              | `a*`              |
| 1 or more   | `a\+`             | `a+`              |
| 0 or 1      | `a\?`             | `a?`              |
| exactly n   | `a\{n\}`          | `a{n}`            |
| n to k      | `a\{n,k\}`        | `a{n,k}`          |
| -           | `\(abc\)`         | `(abc)`           |
| class       | `[abc]`           | `[abc]`           |
| neg class   | `[^abc]`          | `[^abc]`          |
| -           | `^line`           | `^line`           |
| -           | `line$`           | `line$`           |
| ignore case | -i/--ignore-case  | -i/--ignore-case  |
| char. class | `[[:classname:]]` | `[[:classname:]]` |
