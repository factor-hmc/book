## Future Work

Broadly speaking, future work for this vocabulary can be broken up
into two major areas: improving `tensor`s’ performance and usability.

There is still room for optimization with the currently implemented
words. Many of the implemented words are both more than an order of
magnitude slower than NumPy and do not scale as efficiently. In
addition, the performance of a number of `sequence` words can be
improved by providing `tensor`-specific implementations that take
advantage of the underlying structure of the `tensor`. Finally, the
`tensors` vocabulary currently only supports 32-bit floats, and
allowing `tensor`s to store different types of numbers, including
integers and floats of different sizes would allow for additional
performance gains where floating point arithmetic is either not
necessary or not necessary at that precision.

To improve the usability of the `tensors` vocabulary, there are a
number of crucial features implemented with NumPy arrays that the
`tensors` vocabulary does not provide. These include broadcasting,
performing operations over specific axes, index slicing, and more
complicated mathematical operations. The versatility that these
operations provide would make using `tensor`s much easier and more
frictionless.

Finally, the current vocabulary does not always do a good job of
hiding the underlying implementation from the user. Specifically, this
becomes a problem when trying to understand error messages. The
addition of better error checking—both more specific errors and
checking for errors earlier within operations—would make the
vocabulary more intuitive and easier to use.
