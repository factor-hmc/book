## Super_Hello_World
Like most tutorials for a new programming language, we will write a program that prints "Hello World!".  In Factor, writing this program is easy! All we need to type is:

`"Hello World" print`

This passes the string "Hello World" to the word**print**  However, this is too easy.  Let's write a program which can let the user print "Hello_World" as many times as they want.  We will also print out a number right after each "Hello world" to prove we are printing out the correct number of strings.  To begin, let's create a program that prints out "Hello World" multiple times.  Let's say we want to print out "Hello World" 5 times.  One way we could do this is to write:

`"Hello World" dup dup dup dup print print print print print`

The word **dup** in this code duplicates the "Hello World" string.  Since we have **dup** 4 times, we have a total of 5 "Hello World" strings on the stack. Each **print** prints out a single "Hello World" string (this also removes it from the stack).

While this code does accomplish the task of printing "Hello World" 5 times, it is very messy.  What we really want is some way to repeat the code `"Hello World" print`.  Luckily, Factor gives us access to quotations and combinators.  A quotation is a piece of code that we treat as an individual unit that can be moved on the stack, and a combinator is a word which accepts a quotation as an input.  We can turn `"Hello World" print` into a quotation by writing:

`[ "Hello World" print ]`

If we run this code, the quotation will be pushed to the stack.  Since we want to run this code multiple times, we can use the **times** combinator to accomplish this.  The **times** combinator takes in a number for the amount of times we want to run the quotation, and then the actual quotation.  Since we want to print "Hello World" 5 times, lets run the code:

`5 swap`

This should put 5 on the stack and swap the position of 5 and our quotation.  Now that they are in the right order, we can simply run

`times`

and see our program print "Hello World" 5 times.  Now that we understand an efficient way to print "Hello World" any number of times. Let's write this code as a word so we can call it any time.  To do this, we can write:

`: Super_HW ( n -- ) [ "Hello World" print ] times ;`

We see that the name of our function is "Super_HW" and notice that it expects 1 input by looking at the phrase "( n -- )".  We can call our function by writing an input, a space, and then the function name :

`7 Super_HW`

This should print "Hello World" 7 times.  Now, lets update our function so that it counts how many times we print "Hello World".  In order to do this, we need a way iterate through a list of numbers.  We can accomplish this, by creating a range of numbers with the word **[a,b]** in the math.ranges vocabulary.  To do this, let's import the math.ranges vocabulary by typing:

`USE: math.ranges`

Now, we should have access to the word **[a,b]** .  This word excepts two inputs, a lower bound and upper bound.  Our lower bound will always be 1 since we will start counting at 1, however our upper bound will be the user's input.  This world excepts the lower bound first, so we might need to utilize a **swap** to ensure that the inputs are in the proper order.  Now that we understand how to create a range of numbers, we can use a new combinator called **each** which applies a quotation to each element in a range.  This means that for each number in the range, we will run `"Hello World" print` with that number as an input.  We will want the print out that number.  We can do this with the world `.` We can simply add this word to our quotation:
`[ "Hello World" print . ]`Let us update our word to utilize the new range word, combinator, and quotation:

`: Super_HW ( n -- ) 1 swap [a,b] [ "Hello World" print . ] each ;`

Now if we run this word we should see it enumerate our print outs:

`8 Super_HW`

It works!  We were not only able to say "Hello World", but we were able to say it as many times as we wanted with each print out numbered!
