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
