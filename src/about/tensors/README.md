# Numerical Programming

In the past five to ten years, the use of machine learning and, more
specifically, neural networks, has rocketed. Many companies and
research groups are looking to apply these techniques to their
fields. However, these methods often take too long to run, and
programmers implementing such algorithms have to ask themselves if
machine learning is worth the time both at training and inference
time.

To solve this, people create specialized numerical programming
libraries, which allow for the manipulation of many numbers at once
through the use of matrix operations. Some examples of libraries for
numerical programming are [NumPy](https://numpy.org/) and
[SciPy](https://www.scipy.org/). Both are incredibly popular and have
contributed to Python’s popularity as a language.

The goal of this project was to add in Factor the infrastructure
necessary to support numerical programming techniques, including but
not limited to shaped arrays and scalable matrix operations. While
Factor has the ability to perform fast operations, this is not
utilized to perform matrix manipulations. Before this project, there
was not a vocabulary that allowed for fast operations on n-dimensional
arrays.


