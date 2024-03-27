# POSIX.2 Regular Expressions

This is a compilation of the **POSIX.2 Regular Expressions manual page** *(7
regex)* in an easier to read format.

## Regular Expression

A regular expression is one or more nonempty branches, separated by a `|`. It
matches anything that matches one of the branches.

### Branch

A branch is one or more pieces, concetenated. It matches for the first, followed
by a match for the second and so on.

### Piece

A piece is an atom possibly followed by a single `*`, `+`, `?`, or bound. An
atom followed by a:

- **`*`** matches a sequence of 0 or more matches of the atom.
- **`+`** matches a sequence of 1 or more matches of the atom.
- **`?`** matches a sequence of 0 or 1 matches of the atom.

### Bound

A bound is a `{` followed by an unsigned decimal integer, possibly followed by
`,` possibly followed by another unsigned decimal integer, always followed by
`}`.

The integers must lie between `0` and `RE_DUP_MAX` *(255)* inclusively, and if
there are two of them, the first may not exceed the second.

An atom followed by a bound containing one integer `i` and no comma matches a
sequence of exactly `i` matches of the atom.

An atom followed by a bound containing two integers `i` and `j` matches a
sequence of `i` through `j` inclusively matches of the atom.

### Atom

An atom is:

- **A regular expression enclosed in `()`** matching a match for the regular
  expression.
- **An empty set of `()`** matching the null string.
- **A bracket expression**.
- **`.`** matching any single character.
- **`^`** matching the null string at the beginning of a line.
- **`$`** matching the null string at the end of a line.
- **`\` followed by one of the characters: `^ . [ $ ( ) | * + ? { \`** matching
  that character taken as an ordinary character.
- **`\` followed by any other character** matching that character taken as an
  ordinary character, as if the `\` had not been present.
- **A single character with no other significance** matching that character.
- **`{` followed by a character other than a digit** is an ordinary character,
  not the beginning of a bound.
