## Goals and design decisions

Factor, in its current form, is a self-hosted, compiled language.  In
other words, its compiler is written in Factor as well.  This form
doesn't lend itself well to being ported online for two reasons:
- first, the compiler outputs machine code (e.g., x86) meant for
  physical CPUs, not Javascript (or even WebAssembly), the language
  needed for web applications;
- second, in order to port the Factor compiler to Javascript, we would
  first have to port Factor to Javascript, since Factor's compiler is
  written in Factor.  These two reasons meant that we couldn't get
  away with something as simple as "use a X-to-Javascript transpiler
  that someone else wrote" to translate the Factor interpreter to
  Javascript; instead, we would have to understand the mechanics of
  Factor and write our own Factor interpreter—from scratch—in a
  web-friendly language (Javascript, or something that compiles to
  Javascript).

Given that requirement, we recognized it would not be realistic to
fully replicate the behavior of Factor within our one-school-year
timeline.  Thus, keeping in mind that the intent of this project was
to make Factor more beginner-friendly, we prioritized the following
goals:
- implement a core subset of Factor functionality, enough to
  demonstrate what "Factor is like";
- provide features that make it easy and fun to learn Factor (e.g.,
  helpful error messages, interactivity);
- supplement the interpreter with tutorials and
  documentation-searching tools (more in the Tutorials and Foogle
  sections), à la online coding course platforms.  Furthermore, we
  decided to implement the interpreter in [Elm](https://elm-lang.org/), a functional
  programming language for web applications known for its type safety
  (virtually no runtime errors!), performance, and work in
  user-friendly compilers and error messages.
