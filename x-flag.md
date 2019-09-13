## A new flag (x+code…+) – gather of information from emails

The full semantics is:

1. A general form of the substitution is: `${(x+code…+)…}`, where + denotes the
   delimeter. The last `…` denotes the input substitution's value, e.g.
   `${(x+…+)array}` or `${(x+…+):-string}`, or `${(x+…+):-${(s: :)string}}`,
   etc.

1. On entry, `REPLY` is set to one element from the input substitution's value and
   `reply` is unset.

1. If the code returns nonzero (false, failure) then the result is the empty
   array (so the corresponding element is removed from the input set)

1. …else if `reply` has become set, that array is used (even if empty) (so the
   corresponding element may become zero, one, or more elements)

1. …else if `REPLY` is set, the value of `REPLY` is used (so the corresponding
   element changes, possibly to an empty string)

1. …else the original element is unchanged

## Example uses

Extending the input array:

```zsh
array( "a" "b c" "d" )
func() { reply=( ${=REPLY} ); }
array=( ${(x^func^)array} )
print -rl "${array[@]}"

Output:
a
b
c
d
```

Contracting the input array:

```
array( "a" "b c" "d" )
func() {
  if (( ${#${=REPLY}} > 1 ));then
    reply=()
  else
    reply=( $REPLY )
  fi
}
array=( ${(x^func^)array} )

Output:
a
b
```

<!-- vim:set ft=markdown tw=80 fo+=an1 autoindent: -->
