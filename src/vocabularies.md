## Vocabularies
Many programming languages offer users access to a collection of files, functions, or programs called libraries.  In Factor, collections of functions (or words) are **vocabularies**.  Factor has a large amount of vocabularies that users can utilize right off the bat.  However, Factor offers a wide variety of vocabularies that users can import into their program.  To do this, a user must type `USE: ` and then a vocabulary name.  For instance, if one wanted to important the random vocabulary, they would type:

```factor
USE: random
```

This would then allow the user to use words from the random vocabulary.  If one wanted to import multiple vocabularies at once.  We would simply write `USING: ` instead of `USE: `, and type the vocabularies we want to use with a space between each vocabulary and put a space and a semicolon at the end.  This is what importing the entire random and math vocabularies would look like:

```factor
USING: random math ;
```

If you are writing a vocabulary, you would type `IN: ` followed by your vocabulary's name.  For instance, if I was writing a vocabulary called “baseball” and it used the vocabularies math and random. You would write:

```factor
USING: random math ;
IN: baseball
```

Then one would write all the words the vocabulary contained beneath it.