Motivation (general audience)
* Numerical Programming is the practice of using matrices to manipulate many numbers at a time. This is used in machine learning, where many operations have to be performed in an efficient way. 
* While Factor has the ability to perform fast operations, this is not utilized in the area of matrix manipulation. Before this project, there was not a vocabulary that allowed for fast operations on n-dimensional arrays.

Tensors Vocabulary (summary, general audience)
* The Tensors vocabulary can create and then manipulate n-dimensional arrays quickly. Documentation for the Tensors vocabulary can be accessed online here: https://docs.factorcode.org/content/article-tensors.html
* Tensors supports element-wise operations, matrix multiplication, transposition, and linear regression. It is also integrated with the sequences vocabulary and as such can support sequence operations like map and nth (full Sequences documentation here: https://docs.factorcode.org/content/vocab-sequences.html)

Method (how it’s implemented specific audience)
* In order to achieve the desired speed and user experience, we utilized a number of Factor language features.
* To improve the performance, we used specialized (c-type) arrays and typed words to remove the need to check the type of every element of a tensor at runtime, and SIMD operations, which allowed us to perform vectorized computations.
* To make the vocabulary easier to use, we used multimethod dispatch to allow users to combine operations on tensors and constants and implemented the basic words necessary to apply sequence operations and pretty printing

Evaluation (graphs & stuff, medium audience)
* Our goal was to have all operations run within an order of magnitude of NumPy. We reached this goal with some operations, but struggled with others. Continuing to optimize the vocabulary is an area for future work
* First, we benchmarked individual operations (element-wise addition, matrix multiplication, transposition - show graphs)
* Combined all operations to perform linear regression on the Boston housing dataset (https://www.cs.toronto.edu/~delve/data/boston/bostonDetail.html) (show graphs here too)

What’s Next & Challenges for Future Devs (specific audience)
* The operations can always be optimized—the aim is for this vocabulary to be as fast as possible
* Broadcasting (copying numbers down a matrix to do operations between matrices of different sizes)-- instead of just throwing errors after encountering shape mismatches, element-wise operations and matrix multiplication should try to broadcast
* Operations over specific axes, rather than over the entire tensor-- something like map but with tensor-specific inputs
* More complicated mathematical operations (look to numpy for inspiration)
* Allow for tensors to store different types of numbers, including integers and floats of different sizes
* Better error checking-- both more specific errors and checking for errors earlier within operations (some throw errors from words called during the operation, which makes less sense to the user)
