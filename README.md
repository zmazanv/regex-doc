# POSIX.2 Regular Expressions

This is a compilation of the **POSIX.2 Regular Expressions manual page** *(7 regex)*
in an easier to read format.

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
