## Methods and Design Decisions

### Existing Matrix Manipulation Vocabularies

The main matrix manipulation vocabulary in Factor called `math.matrices` provides many of the operations required for numerical programming. However, because the matrices manipulated by this vocabulary are Factor `array`s, operations across these matrices can be slow. Additionally, Factor already has a vocabulary called `arrays.shaped` that contains some of the functionality we hoped to achieve.

However, neither vocabulary incorporates Factor’s static typing or allows for n-dimensional matrices.  Without typed words, operations such as element-wise addition must check the type of each entry in the matrix before performing combining them, leading to a lot of unnecessary work. Because we did not want to overwrite the existing vocabularies, we created a new Factor vocabulary called `tensors` to implement basic shaped arrays functionality.


### Choosing an Implementation Language

We decided to model our approach off of NumPy, given its prominence as a numerical programming library. We had two options for carrying out this project. We could either implement the vocabulary in native Factor or implement it in C and use Factor’s foreign function interface to access the C functions. The latter is the procedure used by libraries such as NumPy and SciPy. We benchmarked vector operations in Factor against those in C. Unoptimized Factor is much slower than C. However, we found that typed Factor, while still slower, can perform operations faster than unoptimized C code and within an order of magnitude of optimized C.

![Factor vs. C](factor_vs_c.png)

In order to use Factor’s foreign function interface (FFI), the C code must be compiled into a library file and included with the Factor code. However, different operating systems require different library file formats, so such an implementation would require users to compile C code themselves. Because Factor runs in a virtual machine, neither programmers nor users have to deal with compiling the code to a specific operating system—it’s done automatically. If we implemented our vocabulary in C, it would require a lot of additional work for our vocabulary to be as accessible as the rest of the Factor source code. Additionally, implementing our vocabulary in Factor also allows us to use more high-level language features, such as automatic garbage collection and higher-order functions like `map` and `reduce`. We decided that the small performance improvement provided by using C would not be worth the loss of portability or ease of use.

### Optimizing Native Factor

The underlying implementation of the `tensor` class consists of an `array` that holds the shape, and a `float-array` that holds the values. Using a typed array increases speed as it avoids type checking at runtime. Additionally, many of the words use strongly-typed word definitions. This alone allowed for a huge speedup over dynamically typed arrays.

To further optimize, we used SIMD operations, which allowed us to perform vectorized computations. This was used to improve the performance of element-wise arithmetic operations such as `t+`, sequence operations such as `sum`, and matrix multiplication.

To read more about how the vocabulary performed, including how it compared to NumPy and the existing `math.matrices` vocabulary, see the [evaluation](evaluation.md) section.

### Interface

One of our main goals was ease of use. To do this, we tried to make a flexible and simple interface. We used multi-method dispatch when implementing element-wise operations so that they can take either two `tensor`s, or a number and a `tensor` in either order. 

For example, the following code snippets all produce an output of `t{ 5 6 7 }`:
- `t{ 0 1 2 } 5 t+`
- `5 t{ 0 1 2 } t+`
- `t{ 0 1 2 } t{ 5 5 5 } t+`

We also implemented the basic words necessary to apply sequence operations and pretty printing. 

`tensor`s can be turned to and from nested arrays using a parsing word. Initialization of a one-dimensional `tensor` using the parsing word `t{` was shown above. A two-dimensional `tensor` can be initialized as follows: `t{ { 0 1 } { 2 3 } }`.

The features implemented during this project cover only a small fraction of the features provided by a library like NumPy. To read more about possible future directions for this project, see the [future work](future.md) section.
