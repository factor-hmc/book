# The Factor Programming Language

Hello!

I can do *italics* and **bold**.

What about `code` and 

```factor
code
```
?

nice.

- bullets
- bullets
  1. nums
  2. nums
  
## secondary

> quote
> quote


link to [page](a.md)

factorial implementation
```factor
: fact ( n -- n! ) dup [ dup 1 = f = ] [ 1 - dup rot * swap ] while drop ;
```
