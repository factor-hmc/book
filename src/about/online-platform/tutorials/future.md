## Future work

There are several aspects of our collection of tutorials that we would
like to improve.  The first is that we would like to create more
tutorials which expand on the basic ideas we have taught and write
more complicated programs like the Super Hello World tutorial.  We
think that showcasing the creation of more complicated projects will
not only show new Factor programmers how to approach building a larger
Factor program, but show off what Factor is capable of. For example, a tutorial which involves creating a simple vocabulary that contains several words or a tutorial demonstrating how to write a small “choose your own adventure” program that accepts inputs during runtime could be added.  There are even more advanced Factor topics for which it would be nice to have tutorials, like how to write unit tests or how to set up a class.

The second addition we would make is to ensure that every line of code
presented in the tutorial can be run in the interpreter.  As of right
now, most of the code in the tutorials can be run on the
interpreter.  However, there are still some Factor features which
are not implemented in the interpreter, meaning that
following the tutorials requires installing Factor.  One tutorial that suffers from this is the vocabularies tutorial.  The interpreter does not let a user run the ```USE:``` word to import any vocabularies.  In the Super Hello World tutorial,  we run into a similar problem when we try to create the word ``` : Super_HW ( n -- ) 1 swap [a,b] [ "Hello World" print . ] each ; ``` which requires the use of the word ```[a,b]``` from the `math.ranges` vocabulary and a combinator ```each``` which is from the sequences vocabulary.  It would make the learning experience much smoother
if this issues were fixed and we could type every line straight into the online interpreter.

Another thing we would like to incorporate into our tutorials is user
feedback.  Our tutorials have yet to be tested on new Factor learners.
We believe that giving these tutorials to new Factor programmers and
then receiving their feedback will allow us to target areas of the
tutorials that are lacking.  One way this could be carried out is to assemble a group of both experienced programmers and newer programmers.  We would then split them into two groups with an even mixture of veteran and novice programmers on each.  Give one group the tutorials from the Factor Online Platform and the other a list of tutorials found elsewhere online.  Allow both groups some time to experiment with the tutorials (maybe an hour).  Then give each group a short quiz that tests their ability to program in Factor as well as a short survey about what they liked and disliked about each tutorial.  This will allow us to see how effective our tutorials our compared to others.  It will also help identity problems with our tutorials.  We can also incorporate other aspects of other tutorials that users found helpful into our own.  A second way to get feedback would be to create a feedback form on the Factor Online Platform where users could suggest improvements on the tutorials and other aspects of the platform.
