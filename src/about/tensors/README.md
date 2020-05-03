# Numerical Programming

In the past five to ten years, the use of machine learning and, more specifically, neural networks, has greatly increased. Many companies and research groups are looking to apply these techniques to their fields. However, these methods often take too long to run, and programmers implementing such algorithms have to ask themselves if machine learning is worth the time and resource cost.

To solve this, people create specialized numerical programming libraries, which allow for the manipulation of many numbers at once through the use of matrix operations. Some examples of libraries for numerical programming are [NumPy](https://numpy.org/) and [SciPy](https://www.scipy.org/). Both are incredibly popular and have contributed to Python’s popularity as a language.

The goal of this project was to add in Factor fast operations on n-dimensional arrays; Factor’s version of NumPy. While Factor has the ability to perform fast operations, this was not previously utilized to perform matrix manipulations. We added the infrastructure necessary to support numerical programming techniques, including but not limited to shaped arrays and scalable matrix operations.

To read more about how we implemented the vocabulary, see the section on [methods and design decisions](methods.md). To read more about how the vocabulary performed, including how it compared to NumPy and the existing `math.matrices` vocabulary, see the [evaluation](evaluation.md) section. To read more about possible future directions for this project, see the [future work](future.md) section.

Documentation for the `tensors` vocabulary can be found [here](https://docs.factorcode.org/content/article-tensors.html).
