## Background and motivation

Have you ever implemented a function that you know must exist already?
You just couldn’t find what library it was in and what that library
called it. This is unsurprising. Even common operations like getting
the first element of a list can have different names between
programming languages:
[first](https://docs.factorcode.org/content/word-first,sequences.html)
in Factor,
[head](https://hackage.haskell.org/package/base-4.12.0.0/docs/Data-List.html#v:head)
in Haskell, or
[car](https://www.gnu.org/software/emacs/manual/html_node/eintr/car-_0026-cdr.html)
in Lisp.

Foogle is inspired by Hoogle, a function search engine for
Haskell. Hoogle allows users to query functions based on the types of
their inputs and outputs. Factor doesn’t require type annotations, but
parameters typically have some amount of type information associated
with them. This either is documented in naming conventions, like `seq`
for `sequence`s or `?` for `boolean`s, or in the help page for the
function, like for
[`+`](https://docs.factorcode.org/content/word-+,math.html) or
[`nth`](https://docs.factorcode.org/content/word-nth%2Csequences.html). In
general, there is some form of identifying information about the
inputs and outputs of a function.
