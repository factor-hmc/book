## Future work & contributing

Below are a few areas in which the online Factor interpreter/platform
needs the most improvement, before the online interpreter can be truly
usable & useful for the general Factor community.

### Compatibility with existing Factor mechanics

Currently, the web-based Factor interpreter is largely an
_approximation_ of how Factor actually works: the subset of Factor
that is supported in the web-interpreter is fairly narrow, and even
among that subset, behavior on the web-interpreter differs (sometimes
in subtle ways) from Factor's current behavior.  Here are a few
examples:

- **Tokenizing and parsing.** In many other languages, syntax for
  special constructs like function definitions and array literals are
  hard-coded into the language's grammar and parser.  On the other
  hand, Factor uses [parsing
  words](https://docs.factorcode.org/content/article-parsing-words.html)
  to handle most of its special syntax: word (function) definitions
  are processed by the parsing words
  [`:`](https://docs.factorcode.org/content/word-__colon__,syntax.html)
  and terminated by
  [`;`](https://docs.factorcode.org/content/word-%3B%2Csyntax.html),
  array literals by
  [`{`](https://docs.factorcode.org/content/word-%7B,syntax.html), and
  so on.  A key benefit of this approach is that it allows endless
  customization of Factor's syntax and the construction of various
  domain-specific languages (DSLs) in native Factor code.  To name a
  few examples:

  - [`pair-rocket`](https://docs.factorcode.org/content/vocab-pair-rocket.html)
    defines
    [`=>`](https://docs.factorcode.org/content/word-%3D__gt__%2Cpair-rocket.html),
    an alternative way to denote pairs in an "associative" container
    (e.g., a dictionary);
  - [EBNF](https://docs.factorcode.org/content/article-peg.ebnf.html)
    parsing words define a DSL that automatically synthesizes
    [PEG-parsers](https://en.wikipedia.org/wiki/Parsing_expression_grammar)
    from grammars expressed in
    [EBNF](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form)
    syntax, reminiscent of parser-generator solutions such as `lex`
    and `yacc` for other languages;
  - [Hash
    tables](https://docs.factorcode.org/content/article-syntax-hashtables.html),
    [hash
    sets](https://docs.factorcode.org/content/article-hash-sets.html),
    [binary search
    trees](https://docs.factorcode.org/content/article-trees.html),
    and in fact _arbitrary_ containers/data structures may be
    constructed via custom-defined literal syntax words,
    e.g. [`H{`](https://docs.factorcode.org/content/word-H%7B,syntax.html),
    [`HS{`](https://docs.factorcode.org/content/word-HS%7B,syntax.html),
    [`TREE{`](https://docs.factorcode.org/content/word-TREE%7B,trees.html),
    etc.
    
  This limitless customization of Factor's syntax and parsing behavior
  is part of what makes Factor a uniquely flexible programming
  language.  So while working with parsing words aren't essential to a
  beginner's understanding _of_ Factor, they arguably _are_ essential
  to an authentic experience of what it is like to work with Factor.
  
  Currently, the web-based interpreter does not use parsing words.
  Instead, it takes the conventional approach and hard-codes a few
  special syntax words (`{` for array literals, `[` for quotation
  literals, `:` for word definitions, etc.) to deal with the most
  common use cases; a more authentic approach would be to replicate
  Factor's actual parser behavior, which may be summarized as follows:

  - Every Factor word is either an _ordinary word_ or a _parsing
    word_.  Ordinary words (e.g., `+`, `print`) are essentially
    non-syntax, callable, "function" words.  Parsing words (e.g. `V{`,
    `::`, `{`, `[`, `=>`), on the other hand, define custom parsing
    behavior, such as special constructs, container literals and DSLs.
    
  - During parsing, Factor keeps an _accumulator vector_, which stores
    a list of "instructions" (e.g., push the number `13` onto the
    stack; push a quotation on the stack; call the word `times` on the
    current stack contents).  The accumulator vector amounts to a
    mutable "syntax tree" for Factor code during parsing.
    
    Given a piece of Factor code, the parser first splits
    the code into tokens by whitespace (the "tokenize" step), then
    processes each token in order.  For each token: 

    - If the token is a literal string or number, a corresponding
      "push" instruction is added to the accumulator vector.  For
      instance, `12` adds a "push the number `12` onto the stack"
      instruction to the accumulator.
    - if the token is an ordinary word, a corresponding "call"
      instruction is added to the accumulator vector.  For instance,
      `+` adds a "call the function `+` on the current stack contents"
      instruction to the end of the accumulator.
    - _If the token is a parsing word_, it is _not_ added to the
      accumulator vector--instead, it is executed immediately, running
      special parsing primitives to parse code occurring after it and
      modifying the accumulator vector accordingly.  For instance, `{`
      interrupts normal Factor parsing, consumes tokens until a `}` is
      found, creates an array with elements initialized from the items
      it parses, and adds a "push this array onto the stack"
      instruction to the accumulator.
      
  You can read more about Factor's parsing mechanics and how parsing
  words work [in the official Factor
  documentation](https://docs.factorcode.org/content/article-parsing-words.html).
  
- **Name resolution and recursion**.  Currently, words are stored
  internally as lists of instructions (push, call, etc.).  A lookup
  table is used during evaluation to map word names to the
  corresponding instructions.  When a new word is defined in terms of
  existing words, we construct the instruction list for the new word
  by _concatenating_ the instruction lists of words used in the
  definition.  For example, if we define
  
  ```factor
  : addmul ( a b c -- d ) + * ;
  ```
  
  then the instruction list for `addmul` is constructed by
  concatenating the instruction list for `+` (obtained via the lookup
  table) with the instruction list for `*`.
  
  There are a few drawbacks to this approach, or at least the current
  implementation:

  - _It's inefficient._ Concatenating lists takes O(n) time
    (w.r.t. the length of the first list--Elm's `List` type is
    implemented internally as a singly-linked list, as in many
    functional languages of the same genre, e.g., Haskell and OCaml)
    and _doubles_ the memory used to store the definitions.  
    
    To illustrate the inefficiency, suppose a word `w` has instruction
    list [i1, i2, ..., in], and we define another word `x` as `w drop`
    (whose instruction list contains a single built-in `drop`
    instruction, since `drop` is a primitive); then `x` will
    instruction list [i1, i2, ..., in, drop].  Note that instructions
    `i1, ..., in` are stored _twice_--once in `w`, and once in `x`.

    The _sensible_ approach is to instead keep a "reference" to words
    used in the definition and call those references, rather than
    "copying" and concatenating the definitions.  A previous attempt
    at implementing this approach involved simply storing the names,
    as strings, of words, and looking up those names when the word is
    called, but that leads to erratic behavior, as in the following
    example:
    
    1. `w` is defined;
    2. `x` is defined in terms of `w`;
    3. `w` is redefined;
    4. when `x` is called, it _should_ use the original/old version of
       `w`, but because resolution happens at call-time, `x` now uses
       the new version of `w`.
       
    Concatenating instruction lists was a quick hack to avoiding the
    above behavior, but it is far from a good solution.  It should be
    noted, also, that the current implementation does not do this
    copy/concatenation recursively: _quotations_ defined in terms of
    existing words are stored using the names of those words, rather
    than concatenations of their instruction lists; this means that
    if, in the above example, the definition `x` uses a _quotation_
    containing `w`, and `w` is redefined, then when `x` is called
    still the new version is used.  So our current solution is really,
    in some sense, the worst of both worlds.
       
  - _Recursion is impossible (sometimes)._ It should be apparent that
    it is impossible to define a word directly in terms of itself
    (recursion from within a quotation is fine, since a quotation
    stores names rather than actual instruction lists), since if we
    were to try to define a word `w` in terms of itself, we would
    first try to find `w` in the table of existing words and
    concatenate `w` to itself--which would fail, since `w` is not yet
    defined!  Or, if `w` were previously defined, redefining `w` in
    terms of "itself" doesn't actually recursively call itself;
    instead, wherever a "recursive" call to `w` is made, the
    _previous_ version of `w` is used instead.
    
  A better way to handle word definitions is to combine lists "by
  reference".  One way to do so while allowing self-referencing
  (recursive) definitions is described below:
  
  - Define an `Instruction` type to be either a `Push` or `Call`
    variant: a `Push` variant denotes "push some item onto the stack",
    a `Call` variant denotes "call this word/instruction list on the
    stack".
    
    - The `Push` variant should hold a single parameter of type
      `Literal`, the object to push onto the stack;
      
    - The `Call` variant would hold a single parameter of type `List
      Instruction`, the instruction list to call on the stack.

      However, in this setup Elm's recursion system would prevent us
      from being able to define a self-referencing word such as `: a (
      -- ) 1 a ;` (yes, that example is one that has no practical use,
      but should still be theoretically possible to define), since
      traditional "tying the knot" methods for mutual/self-recursion
      are disallowed in Elm (read more about the problem
      [here](https://elm-lang.org/0.19.1/bad-recursion)).  The gist of
      the problem is that since `a` is defined in terms of itself, its
      instruction list must contain a `Call` variant holding, well,
      the instruction list itself.  Since Elm evaluates expressions
      strictly, a recursive definition like this would cause Elm to
      recur infinitely.
      
      The solution is to explicitly "lazify" the recursion: instead of
      parametrizing `Call` with a `List Instruction`, we parametrize
      it with _a function_ that lazily returns a `List Instruction`
      when called.  This allows us to apply a tie-the-knot technique
      without sending the Elm evaluator into an infinite recursive
      loop.
      
    The code below illustrates a simplified example of this idea in
    action:
      
    ```elm
    
    type Instruction 
      = Push Int
      | Call (() -> List Instruction)
      
    a : List Instruction
    a = let lazy () = [ Push 1, Call lazy ] in lazy ()
    ```
    
    compared to what the code _would_ look like if Elm supported
    recursive definitions with laziness by default--note that this
    code _would not_ work in Elm:
    
    ```elm
    
    type Instruction
      = Push Int
      | Call (List Instruction)
      
    a : List Instruction
    a = [ Push 1, Call a ]
    ```
  
- **Word definitions.** Currently, word definitions on the web
  interpreter are evaluated at run time, i.e., when a definition
  instruction is executed.  In Factor, word definitions are evaluated
  at parse time.  To illustrate the difference, consider the following
  piece of code, wherein we define a word from within a quotation:
  
  ```factor
  [ : add1 ( n -- m ) 1 + ; ]
  5 add1
  ```
  
  In desktop Factor, `:` is executed as soon as it is encountered
  (since it is a parsing word), parsing and loading the definition
  `add1` immediately.  Thus an _empty_ quotation is pushed on to the
  stack (all tokens were consumed by the parsing process of `:`),
  `add1` is immediately defined and can be subsequently applied on `5`.
  
  On the current web interpreter, `:` is _not_ executed at parse time.
  Instead, it is parsed as a special "definition" instruction and
  stored in the quotation as though it were a special word.  Thus a
  quotation containing the "definition" instruction is pushed onto the
  stack, `add1` would not be defined, and the subsequent `5 add1` call
  would fail because `add1` is not defined.  One would have to `call`
  the quotation to actually define `add1`.
  
  Of course, it could be argued that this method allows for some
  interesting behavior as well--but it is confusing and certainly
  doesn't match the expected Factor behavior.  So it should be
  changed.  Presumably, fixing parsing (see the first bullet point)
  and adding parsing words would fix this issue as well.
  
- **Stack effects.** The current stack effect parser is incomplete: it
  can't handle any "nested" effects (e.g. `( quot: ( a -- b ) seq --
  seq' )` and doesn't parse any special tokens (e.g. `!` for
  mutability, `..` for row polymorphism).  It only accounts for
  space-delimited tokens separated by a `--`.  Furthermore, currently
  nothing useful is done with the stack effect--at a minimum, there
  should be some form of stack-checking/stack-inference used to verify
  word definitions.
  
- **Miscellaneous language features.** Various language features, such
  as classes, tuples, mixins, generics, etc. are not implemented.  The
  absence of these features prevents a significant portion of existing
  Factor code from working in the web interpreter.

### Loading existing Factor vocabularies

Once Factor mechanics are replicated sufficiently in the web
interpreter per the above points, we will be able to properly load
existing Factor vocabularies (e.g., `combinators`, `sequences`,
`math`, etc.).  The proposed method for doing so is as follows:

- Implement a parsing word `USE:`, which calls a primitive "load
  vocabulary" instruction from the Elm-based runtime.
  
- When the "load vocabulary" instruction is called, make an HTTP
  request to the official Factor GitHub repository to fetch the
  corresponding `.factor` source file (not having to keep a separate
  copy/version of the Factor source repository makes the interpreter
  easier to maintain; since the interpreter is meant as an
  experimental scratchpad anyway, version consistency & stability
  aren't that important anyway).  Parse the source file and load it in
  the interpreter runtime.

### General UI and learner-friendly improvements

- The desktop Factor listener is highly interactive and introspective:
  most values are clickable and, when clicked, bring up dialogs
  presenting the contents/other details about the items clicked.  The
  web interpreter lacks this feature.

- We set out to make the web interpreter a low-barrier means for
  Factor newcomers to experiment with and learn Factor; key to this
  experience is helpful error reporting with user-friendly, intuitive
  error messages (see https://elm-lang.org/news/the-syntax-cliff,
  https://elm-lang.org/news/compilers-as-assistants,
  https://elm-lang.org/news/compiler-errors-for-humans).  Currently,
  the web interpreter has _no_ error reporting features.

- Syntax highlighting would be cool to have.
