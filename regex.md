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

| Action        | grep (BRE)         | grep -E (ERE)      | pytest | Notepad++ |
|---------------|--------------------|--------------------|--------|-----------|
| or            | `a\|b`             | `a\|b`             |        |           |
| 0 or more     | `a*`               | `a*`               |        |           |
| 1 or more     | `a\+`              | `a+`               |        |           |
| 0 or 1        |  `a\?`             | `a?`               |        |           |
| exactly n     | `a\{n\}`           | `a{n}`             |        |           |
| n to k        | `a\{n,k\}`         | `a{n,k}`           |        |           |
| -             | `\(abc\)`          | `(abc)`            |        |           |
| class         | `[abc]`            | `[abc]`            |        |           |
| neg class     | `[^abc]`           | `[^abc]`           |        |           |
| start of line | `^line`            | `^line`            |        |           |
| end of line   | `line$`            | `line$`            |        |           |
| ignore case   | -i/--ignore-case   | -i/--ignore-case   |        |           |
| char. class   | `[:classname:]`*   | `[:classname:]`*   |        |           |

*) POSIX Class-name types for grep (see below)

# Character class

For grep, character class: `[:classname:]`
Character classes need to be wrapped in `[]` to give the bracket expression `[[:classname:]]`.
Without the bracket expression `[:digit:]` would just mean match from these characters: `[:dgit]`

`classname` values:

| POSIX class char. name | Meaning            | Direct form                   |
|------------------------|--------------------|-------------------------------|
| digit                  | Digits 0-9         | `[0-9]`                       |
| alpha                  | Alphabetic         | `[a-zA-Z]`                    |
| alnum                  | Alphanumeric       | `[a-zA-Z0-9]`                 |
| lower                  | Lower-case         | `[a-z]`                       |
| upper                  | Upper-case         | `[A-Z]`                       |
| blank                  | Space + tab        | `[ \t]`                       |
| space                  | Space chars.       | `[ \t\n\v\f\r]`*              |
| cntrl                  | Control chars.     | `[\x00-\x1F\x7F]`*            |
| graph                  | Printable + [^ ]   | `[!-~]`* (all ASCI, no space) |
| punct                  | Punctuation        | `[!"#$%&'()*+,\-]`*           |
| xdigit                 | Hexadecimal digits | `[0-9A-Fa-f]`                 |

*) approximation/not easily convertible - don't use them to cover the same range.  
Shown as example what is included.
