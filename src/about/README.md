# About This Project

The goal of this project is to make non-trivial improvements to
Factor. There are many avenues for a concerted effort to grow Factor
in an organized and consistent manner. Any programming language can
benefit from this sort of effort, Factor included. After consulting
with the Factor maintainer [John](https://re-factor.blogspot.com/), we
narrowed down this open-ended goal to two projects: Create an online
platform for learning and using Factor.  Implement a numerical
programming vocabulary in Factor

This page provides summaries and methods for each project, as well as
possible areas for further development.

## The Factor Online Platform

Factor is a great programming language, but has yet to break into the
mainstream. We think that improving the learning process will help to
improve its rate of adoption. Our online platform targets three
aspects that could make the language more inviting to new programmers:
lowering the installation barrier, adding more tutorials and learning
materials, and creating a documentation search engine.

Read more about this project [here](online-platform/README.md).

## The `tensors` Vocabulary

The `tensors` vocabulary was created to provide a fast numerical
programming vocabulary to Factor users. `tensor`s are n-dimensional
arrays that can be efficiently created and manipulated. They support
element-wise operations, matrix multiplication, transposition, and
linear regression. They also implement the `sequences` vocabulary and
support sequence operations such as `map` and `nth`. Documentation for
the `tensors` vocabulary can be accessed online
[here](https://docs.factorcode.org/content/article-tensors.html).

Read more about this project [here](tensors/README.md).
