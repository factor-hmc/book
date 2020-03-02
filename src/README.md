# The Factor Programming Language

Welcome to the Factor Programming Language!  If you are reading this, are you really paying attention to what we're saying?

```factor
: fact ( n -- n! ) dup [ dup 1 = f = ] [ 1 - dup rot * swap ] while drop ;
```
