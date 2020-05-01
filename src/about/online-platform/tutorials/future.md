## Future work

There are several aspects of our collection of tutorials that we would
like to improve.  The first is that we would like to create more
tutorials which expand on the basic ideas we have taught and write
more complicated programs like the Super Hello Word tutorial.  We
think that showcasing the creation of more complicated projects will
not only show new Factor programers how to approach building larger
Factor, but show off what Factor is capable of.  Some potential tutorial ideas which expand on the ideas taught in our previous tutorials could be a tutorial which involves creating a simple vocabulary that contains several words or a tutorial which shows how to write a small “choose your own adventure” program that accepts inputs during runtime.  There are even more advanced Factor topics that could use a tutorial like how to write unit tests, or how to set up a class.

The second addition we would make is to ensure that every line of code
presented in the tutorial can be run in the interpreter.  As of right
now a good chunk of the code in the tutorials can be run on the
interpreter.  However there are still some features of Factor which
have not been implemented into the interpreter meaning if one was to
follow the tutorials, they would need to have installed Factor on
their computer.  One tutorial that suffers from this is the vocabularies tutorial.  The interpreter does not let a user run the '''USE:''' word to import any vocabularies.  Another place that has this issue is in the Super Hello World tutorial.  It runs into a similar problem as we create the word ``` : Super_HW ( n -- ) 1 swap [a,b] [ "Hello World" print . ] each ; ``` which requires the use of the word '''[a,b]''' from the math.ranges vocabulary and a combinator '''each''' which is from the sequences vocabulary.  It would make the learning experience much smoother
if this issues were fixed and we could type every line straight into the online interpreter.

Another thing we would like to incorporate into our tutorials is user
feedback.  Our tutorials have yet to be tested on new factor learners.
We believe that giving these tutorials to new factor programmers and
then receiving their feedback will allow us to target areas of the
tutorials that are lacking.  One way this could be carried out is to assemble a group of both experienced programmers and newer programmers.  We would then split them into two groups with an even mixture of veteran and novice programmers on each.  Give one group the tutorials from the Factor Online Platform and the other a list of tutorials found elsewhere online.  Allow both groups some time to experiment with the tutorials (maybe an hour).  Then give each group a short quiz that tests their ability to program in Factor as well as a short survey about what they liked and disliked about each tutorial.  This will allow us to see how effective our tutorials our compared to others.  It will also help identity problems with our tutorials.  We can also incorporate other aspects of other tutorials that users found helpful into our own.  A second way to get feedback would be to create a feedback form on the Factor Online Platform where users could suggest improvements on the tutorials and other aspects of the platform.
