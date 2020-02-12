# The Factor Programming Language

This is the Factor Programming Language.  There will be better
descriptive text here.  Tutorial sections on the left.

```factor
: fact ( n -- n! ) dup [ dup 1 = f = ] [ 1 - dup rot * swap ] while drop ;
```
