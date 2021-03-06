## Words
As like most Programming languages, Factor gives users the ability to write functions.  In Factor, functions are known as **words**.  Let's take a look at the simple word **add_div** which takes three numerical inputs and divides one input by the sum of the other two:

```factor
: add_div ( a b c -- a/(b+c) ) + / ;
```

Every word has a collection of parts that define them.  Between each section of a word, a space if used to signify that we are defining the next part of the word.  We will now explain what each section of the add_div word does.  We see that typing:

```factor
: 
```

declares the start of a word.  The next part of the word:

```factor
add_div 
``` 

is the word name.  We can use this name to call the function in different parts of our program.  The following section of the word:

```factor
( a b c -- a/(b+c) ) 
```

specifics the inputs and outputs of the function.  The number of inputs is specified by the number of input variable names that are on the left side of the double hyphens.  Each variable input name is separated by a space.  Likewise, the number of outputs a function has is determined by the amount of the output variable names on the right side of the double hyphens.  In this function, we see that it accepts three inputs as three input names are separated by spaces on the left side of the double hyphen.  We also see that there is a single output as 

```factor
a/(b+c)
```

represents a single output name.  The following section:

```factor
+ /
```

describes the operations the function will perform on the inputs.  The function has 3 inputs a, b, and c.  Inputs b and c are pushed onto the stack last, which means that the plus addition operator will sum them.  Now the stack will contain the input a and the sum of b and c.  Next, the division operator will divide a by the sum of b and c.  The following result will be pushed on the stack.  The last part of the word:

```factor
 ;
 ```

is used to signify the end of the word definition.
In conclusion, a word definition has 5 parts, a colon representing the start of a word definition , a word name to use when the word is called, the number of input and outputs a function has in the form `( input_name1 input_name2 input_name3 -- output_name1 output_name2 ) `, the operation to be completed on the input, and a semicolon signifying the end of the definition.