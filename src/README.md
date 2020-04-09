# The Factor Programming Language

Welcome to the Factor Programming Language!

: fact ( n -- n! ) dup [ dup 1 = f = ] [ 1 - dup rot * swap ] while drop ;
