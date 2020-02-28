## The Stack
**Factor** is a stack based programming language.  The stack in **Factor** is an array of dynamically-typed values.  Programs in **Factor** use functions to manipulate the stack.  A **Factor** programmer can utilize the the stack to perform operations quickly. Pushing literals (like 8, and "hello") onto the stack is easy, as literals will push themselves onto the stack when code is run. Notice how running the code:

```factor
8
```

 
adds the literal 8 to the stack.  In order to remove all the items from the stack, we can run the code:

```factor
clear
```

The **clear** function will give us a new stack.  It is very helpful if you have made mistakes and want a fresh start.  

When we push literals to the stack, we are not limited to adding one literal at a time.  Notice how typing the code:

```factor
8 1 4
```

pushes 8, 1, and 4 onto the stack.

Functions  in **Factor** manipulate values on the stack.  For instance, if we run the code:

```factor
+
```

We see that the two most recently pushed values (1, and 4) are removed from the stack, and in their place is their sum.  **Factor** also allows us to apply multiple function at a time.  We can see this in action if we run the code:

```factor
7 - + 
```

on our current stack.  This code pushes 7 to the stack, applies the operation 5 - 7 which removes the literal 5 and 7 from the stack and pushes the result the result of 5 - 7 (-2) to the stack .  Then the program sums 8 + (-2), which removes both values from the stack and replaces them with their sum 6.  Thus, we end up with a stack containing only the literal 6.

**Factor** gives programmers access to a collection of important functions that allow users to shape the stack.  There are a variety of functions that allow us to edit the positions and contents of entries in the stack.  One very useful function is **dup**.  If we run the code:

```factor
dup
```

We see the the value 6 on our stack has been duplicated.  The **dup** function will duplicated the most recent item on the stack, and push the copy on the stack. **Factor** offers many simple, yet powerful functions which can give users more control over the stack.  You can see a brief explanation for some of the more important functions below:

`
dup ( x -- x x ) !Duplicate the most recent value on the stack and pushes the copy to the stack.
drop ( x -- ) !Removes the most recent value from the stack.
swap ( x y -- y x ) !Swaps the order to the last two elements on the stack
over ( x y -- x y x ) !Duplicates the second most recent value and pushes it onto the stack
dupd ( x y -- x x y ) !Duplicates the second most recent value and adds it to the stack as the second most recent value.
swapd ( x y z -- y x z ) !Swaps the posistions of the second and third most recent values on the stack.
nip ( x y -- y ) !Removes the second most recent value on the stack
rot ( x y z -- y z x ) !Makes the third most recent value, the first, the second most recent value the third, and the first most recent value the second.
-rot ( x y z -- z x y ) !Makes the third most recent value the second, the second most recent value the first, and the first most recent value the third.
2dup ( x y -- x y x y ) ! This duplicate the second and first most recent values on the stack and pushes then to the stack in that order.
`