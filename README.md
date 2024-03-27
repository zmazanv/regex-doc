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

### Bracket Expression

A bracket expression is a list of characters enclosed in `[]`.

#### Negation

It normally matches any single character from the list. If the list begins with
`^`, it matches any single character not from the rest of the list.

#### Range

If two characters in the list are separated by `-`, this is shorthand for the
full range of characters between those two inclusively in the collating
sequence. It is illegal for two ranges to share an endpoint. Ranges are very
collating-sequence-dependent, and portable programs should avoid relying on
them.

Examples:

- `[0-9]`: In ASCII matches any decimal digit.
- `[a-c-e]`: Illegal because of the shared endpoint between ranges.

#### Literals

- **To include a literal `]`** in the list, make it the first character,
  following a possible `^`.

- **To include a literal `-`**, make it the first or last character, or the
  second endpoint of a range.

- **To use a literal `-` as the first endpoint of a range**, enclose it in `[.`
  and `.]` to make it a collating element.

- **All other special characters, including `\`**, lose their special
  significance within a bracket expression, with the exception of these and some
  combinations using `[`.

Within a bracket expression, a collating element enclosed in `[.` and `.]`
stands for the sequence of characters of that collating element. The sequence is
a single element of the bracket expression's list. A bracket expression
containing a multicharacter collating element can thus match more than one
character. An example is if the collating sequence includes a `ch` collating
element, then the RE `[[.ch.]]*c` matches the first five characters of `chchcc`.

Within a bracket expression, a collating element enclosed in `[=` and `=]` is an
equivalence class, standing for the sequences of characters of all collating
elements equivalent to that one, including itself. If there are no other
equivalent collating elements, the treatment is as if the enclosing delimiters
were `[.` and `.]`. An example is if `o` and `o` are the members of an
equivalence class, then `[[=o=]]`, `[[=o=]]`, and `[oo]` are all synonymous. An
equivalence class may not be an endpoint of a range.

Within a bracket expression, the name of a character class enclosed in `[:` and
`:]` stands for the list of all characters belonging to that class.

Standard character class names are:

- alnum
- alpha
- blank
- cntrl
- digit
- graph
- lower
- print
- punt
- space
- upper
- xdigit

These stand for the character classes defined in `wctype(3)`. A locale may
provide others. A character class may not be used as an endpoint of a range.

In the event that an RE could match more than one substring of a given string,
the RE matches the one starting earliest in the string. If the RE could match
more than one substring starting at that point, it matches the longest.
Subexpressions also match the longest possible substrings, subject to the
constraint that the whole match be as long as possible, with subexpressions
starting earlier in the RE taking priority over ones starting later. Note that
higher-level subexpressions thus take priority over their lower-level component
subexpressions.

Match lengths are measured in characters, not collating elements. A null string
is considered longer than no match at all. For example, `bb*` matches the three
middle characters of `abbbc`, `(wee|week)(knights|nights)` matches all ten
characters of weeknights, when `(.*).*` is matched against `abc` then the
parenthesized subexpression matches all three characters, and when `(a*)*` is
matched against `bc` both the whole RE and the parenthesized subexpression match
the null string.

If case-independent matching is specified, the effect is much as if all case
distinctions had vanished from the alphabet. When an alphabetic that exists in
multiple cases appears as an ordinary character outside a bracket expression, it
is effectively transformed into a bracket expression containing both cases, for
example, `x` becomes `[xX]`. When it appears inside a bracket expression, all
case counterparts of it are added to the bracket expression, so that, for
example, `[x]` becomes `[xX]` and `[^x]` becomes `[^xX]`.

No particular limit is imposed on the length of REs. Programs intended to be
portable should not try to employ REs longer than 256 bytes, as an
implementation can refuse to accept such REs and remain POSIX-compliant.

Obsolete ("basic") regular expressions differ in several respects. `|`, `+`, and
`?` are ordinary characters and there is no equivalent for their functionality.
The delimiters for bounds are `\{` and `\}`, with `{` and `}` by themselves
ordinary characters. The parentheses for nested subexpressions are `\(` and
`\)`, with `(` and `)` by themselves ordinary characters. `^` is an ordinary
character except at the beginning of the RE or the beginning of a parenthesized
subexpression, `$` is an ordinary character except at the end of the RE or the
end of a parenthesized subexpression, and `*` is an ordinary character if it
appears at the beginning of the RE or the beginning of a parenthesized
subexpression (after a possible leading `^`).

Finally, there is one new type of atom, a back reference: `\` followed by a
nonzero decimal digit **d** matches the same sequence of characters matched by
the __d*th*__ parenthesized subexpression (numbering subexpressions by the
positions of their opening parentheses, left to right), so that, for example,
`\([bc]\)\1` matches `bb` or `cc`, but not `bc`.

#### Collating Element

A collating element is:

- A character.
- A multicharacter sequence that collates as if it were a single character.
- A collating-sequence name for either.
