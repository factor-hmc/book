## Future work

Here is a list of things we wanted to do but couldn't get around to:
- Loading standard vocabularies into the interpreter (e.g.,
  `sequences`, `math`, `arrays`, etc.)
- Advanced parsing using the `Parser.Advanced` module, which should
  make it easier to track parsing "context" and provide more helpful
  error messages
- Stack checking/stack inference
- Support for custom syntax words (declared using `SYNTAX:`; e.g., `H{
  }` for hash tables, etc.)
- Extending the web Factor parser/runtime to match actual Factor
  mechanics more closely (certain features such as recursion & name
  resolution don't match actual Factor behavior)
- Clickable/introspectable UI (a la the desktop Factor listener)
- Smart error reporting (see
  https://elm-lang.org/news/the-syntax-cliff,
  https://elm-lang.org/news/compilers-as-assistants,
  https://elm-lang.org/news/compiler-errors-for-humans)
- Colors, syntax highlighting, formatting
- Better URL handling/navigation
