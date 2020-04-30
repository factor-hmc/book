## Evaluation

In addition to operations over `tensor`s being correct, it was important that they were fast—fast enough that they were useful in situations requiring large matrices. Quantifying this more specifically, our goal was to have all operations run within an order of magnitude of NumPy, which is the gold standard for numerical programming libraries. We also compared the performance of our vocabulary to the `math.matrices` vocabulary in order to understand what improvements we were actually providing the language.

### Individual Operations

First, we benchmarked three individual operations: element-wise addition, matrix multiplication, and transposition. The performance of the `tensors` vocabulary came within an order of magnitude of NumPy with some operations, but struggled with others. To read more about how we achieved the performance gains we did, please see the [methods and design decisions](methods.md) section.

![Add Two 100k Element Arrays](add.png)

![Multiply Two 100x100 Element Arrays](matmul.png)

![Transpose a 100x100 Element Array](transpose.png)

Transpose in particular is much slower than both NumPy and `math.matrices`. NumPy has constant-time transposition, so we did not expect to always be within an order of magnitude of its speed. However, we did want our transposition to be faster than `math.matrices`. We believe that the speed of `math.matrices` is due to the fact that the underlying implementation consists of nested arrays rather than a single array, and is therefore easier to map over. Furthermore, `tensor` transposition must be generalizable to any dimension. Still, the current performance is not ideal, and improving the speed of our transposition is an important next step.

### Proof of Concept: Linear Regression

Additionally, as a proof of concept for our vocabulary, we combined a series of operations to perform linear regression on the [Boston housing dataset](https://www.cs.toronto.edu/~delve/data/boston/bostonDetail.html). The implementation can be found [here](https://github.com/factor/factor/blob/master/extra/tensors/demos/demos.factor).

Linear regression was the perfect proof of concept since it used all of the features that we had implemented, including element-wise arithmetic operations, matrix multiplication, and transposition. For only one step—normalizing the features individually—did we need to deviate from the provided functionality. 

![Linear Regression on the Boston Housing Dataset](linear_regression.png)

As we see in the graph above, even on this larger integration test, the `tensors` vocabulary performed within an order of magnitude of NumPy on the given dataset, meeting our goal for the project. That being said, it is still more than six times slower, and there is definitely more work to be done in this area. To learn more about what can be done in the future to continue to improve these metrics and add the missing functionality, please see the [future work](future.md) section.
