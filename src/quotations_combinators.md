# Quotations and Combinators

**Quotations** and **Combinators** are two important tools which factor offers.  **Quotations** are snippets of code that one can push onto the stack.  These snippets of code take up a singular space on the stack when pushed.  A **Combinator** is a word that accepts a quotation as an input.  Using these tools together can greatly empower a factor programmer.

First, we will learn how to write a quotation.  Do to this, we must bracket the desired code snipet we want to push, with square brackets.  For instance, to write a quotation for the code snippet `+`, we can write:
```factor
[ + ]
```

Now we can use combinator like, **times** or **dip**
to take in our quotations in order to perform operations.  Combinators and quotations can help to create pieces of code that are essentially loops.  Let's create a word that finds a value in the Fibonacci sequence (the first 1 will be our 0th term in the example.)  To accomplish this, we will want to use the **times** combinator which takes in a number and a quotation.  Our quotation will assume that the previous 2 entries of the fibonacci sequence are on the stack.  We will want to create the next entry, as well as preserve the previous entry.  Thus our qutotation will be `[ dup swapd + ]`.  The `dup` will dulicate the previous term so that we can use it in finding the the next term.  The **swapd** will move rotate our 3 numbers on the stack, so that one of the copies of our current largest term is not used in the `+` operation can can be used in finding the next term.  The `+` operation will then combine the 2 largest numbers in the Fibonacci sequence and add that number to the stack.  We then need our function to start with the first two terms of the Fibonacci sequence (0 and 1) on the stack.  This is to ensure our input is moved so that it is used in the times combinator (we can use the word `swap` for this). We also want to drop any unnecessary terms from the stack at the end of our operations. In the end our, function might look something like:

```factor
: Fib ( n -- n ) 0 swap 1 swap [ dup swapd + ] times swap drop ;
```

We see that we push 0 and 1 to the stack and use `swap` to give our input along with the quotation to the `times` combinator.  After we find the desired term in the sequence, we use `swap drop` to drop the unnecessary term that could be used to find larger Fibonacci numbers.  Typing:

```factor
5 Fib
```
gives use the value 8, which is the 5th term if we count the first 1 as the 0th term.  Thus, we see how powerful combinators, and quotations are when programming in factor.  
