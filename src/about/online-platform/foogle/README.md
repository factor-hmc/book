# Foogle

Suppose you’re writing a program in Factor and you have a list which you need to get the 7th element of. You don’t know how to do this in Factor, but you suspect there is a function that you can use. Foogle can find this function for you. You give it the query

```
( n:number seq:sequence -- elt )
```

and it gives you back a bunch of functions, among them `nth`, the function you’re looking for.

Foogle works by taking information about the inputs and outputs of a function. In this case, you’ve specified that your function’s first input (`n`) is a `number` and its second input (`seq`) is a `sequence`. You don’t know any specific information about its output, so you give it a name (`elt`) but don’t specify what type it has to be. Names are still important, since even if you don’t have type information about an input or output, if you have a sense of what its name might be, that will help Foogle find it. Using this information, Foogle finds functions in Factor that you might be looking for.

Try it at [https://factor.netlify.com/foogle](https://factor.netlify.com/foogle).
