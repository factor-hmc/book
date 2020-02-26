## Vocabularies
Many programming languages offer users access to a collections of files, functions, or programs called libraries.  In Factor, collections of functions (or words) are **vocabularies**.  Factor has a large amount of vocabularies that users can utilize right off the bat.  However Factor offers a wide variety of vocabularies that users can import into their program.  To do this, a user must type `USE: ` and then a vocabulary name.  For instance, if one wanted to important the random vocabulary, they would type:

`USE: random`

This would then allow then to use words from the random vocabulary.  If one wanted only a specific word from a vocabulary, they can write the vocabulary with a period at the end and then write the name of the word.  This is how one would import the `random-32` word from the random vocabulary:

`USE: random.random-32`

We can all import multiple vocabularies at once.  We simply write `USING: ` instead of `USE: `, put a space between each vocabulary and put a semi colon at the end.  This is what importing the entire random and math vocabularies would look like:

`USING: random math ;`

We can even import specific words the same way.  This is what importing the words random-32 from the random vocabulary and complex from the math vocabulary look like:

`USING: random.random-32 math.complex ;`

If you are writing a vocabulary, make sure to type `IN: ` followed by your vocabulary's name to allow your vocabulary to utilize the imported words and vocabularies.