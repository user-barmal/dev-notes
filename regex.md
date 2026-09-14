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

| Action          | grep (BRE)                  | grep -E (ERE)               | pytest           | Notepad++ |
|-----------------|-----------------------------|-----------------------------|------------------|-----------|
| escape char.    | `\`                         | `\`                         | `\`              |           |
| alteration (or) | `a\|b`                      | `a\|b`                      | `a|b`            |           |
| 0 or more       | `a*`                        | `a*`                        | `a*`             |           |
| 1 or more       | `a\+`                       | `a+`                        | `a+`             |           |
| 0 or 1          |  `a\?`                      | `a?`                        | `a?`             |           |
| exactly n       | `a\{n\}`                    | `a{n}`                      | `a{n}`           |           |
| n or more       | `a\{n,\}`                   | `a{n,}`                     | `a{n,}`          |           |
| n to k          | `a\{n,k\}`                  | `a{n,k}`                    | `a{n,k}`         |           |
| groups          | `\(first\|second\)`         | `(first|second)`            | `(first|second)` |           |
| class           | `[abc]`                     | `[abc]`                     | `[abc]`          |           |
| neg class       | `[^abc]`                    | `[^abc]`                    | `[^abc]`         |           |
| range           | `[a-zA-Z0-9]`               | `[a-zA-Z0-9]`               | `[a-zA-Z0-9]`    |           |
| start of line   | `^line`                     | `^line`                     | `^line`          |           |
| end of line     | `line$`                     | `line$`                     | `line$`          |           |
| ignore case     | -i/--ignore-case            | -i/--ignore-case            |                  |           |
| char. class     | `[:classname:]`<sup>1</sup> | `[:classname:]`<sup>1</sup> |                  |           |

[1] POSIX Class-name types for grep (see below)
[X] Can be extended by adding OR `|` inside.
[Y] Can be extended by adding quantifiers `*+?` to it.
[Z] Escape character is used to be able to search for characters that have special usage in regex:  
`\[ \] \( \) \\ \*` and more.

# Character class

For grep, character class syntax is: `[:classname:]`  
Character classes need to be wrapped in `[]` to give the bracket expression `[[:classname:]]`.
Without the bracket expression `[:digit:]` would just mean match from these characters: `[:dgit]`  
Because they are intened to be used inside the `[]`, they can be mixed: `[0-5[:punct:]a-f[:blank:]]`

`classname` values:

| POSIX class char. name | Meaning            | Direct form                    |
|------------------------|--------------------|--------------------------------|
| digit                  | Digits 0-9         | `[0-9]`                        |
| alpha                  | Alphabetic         | `[a-zA-Z]`                     |
| alnum                  | Alphanumeric       | `[a-zA-Z0-9]`                  |
| lower                  | Lower-case         | `[a-z]`                        |
| upper                  | Upper-case         | `[A-Z]`                        |
| blank                  | Space + tab        | `[ \t]`                        |
| space                  | Space chars.       | `[ \t\n\v\f\r]`<sup>1</sup>    |
| cntrl                  | Control chars.     | `[\x00-\x1F\x7F]`<sup>1</sup>  |
| graph                  | Printable + [^ ]   | `[!-~]`* (all ASCI, no space)  |
| punct                  | Punctuation        | `[!"#$%&'()*+,\-]`<sup>1</sup> |
| xdigit                 | Hexadecimal digits | `[0-9A-Fa-f]`                  |

[1] approximation/not easily convertible - don't use them to cover the same range.  
Shown as example what is included.

# Python regex shorthand character classes

They differ from grep classes with being Unicode-aware.

\d - digit
\D - non-digit
\w - word character
\W - non-word character
\s - whitespace
\S - non-whitespace
\b - word boundary
