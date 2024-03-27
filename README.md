# POSIX.2 Regular Expressions

This is a compilation of the **POSIX.2 Regular Expressions manual page** *(7 regex)*
in an easier to read format.

## Regular Expression

A regular expression is one or more nonempty branches, separated by a `|`. It
matches anything that matches one of the branches.

### Branch

A branch is one or more pieces, concetenated. It matches for the first, followed
by a match for the second and so on.
