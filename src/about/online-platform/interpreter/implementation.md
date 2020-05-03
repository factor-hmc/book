## The implementation

As we mentioned in the previous section, our online Factor interpreter
is written in Elm.  This section will discuss, at a high level, the
architecture and the parts that fit together to make it work.  To get
into the finer details, you can find our source code [on
GitHub](https://github.com/factor-hmc/simple-interpreter).

### The Elm architecture

If you're not familiar with the Elm architecture, it may be a good
idea to [read about that](https://guide.elm-lang.org/architecture/)
first (it's the same "architecture" that inspired the design of
React/Redux, Vue/Vuex, and many other modern Javascript web
frameworks).  The gist of the Elm architecture is this: the
entire application state (or "model"; this is stuff like the contents
of every text field, the data to be displayed, the currently focused
tab, etc.) is stored globally. Every time an event happens (e.g., a
button is clicked, the contents of a text field changes, an API call
returns a result), a "message" is emitted, and the model is updated
using an `update` function. This takes in a
message and the current model and returns the _next_ model, or how the
model should look after the message/event occurs.  Each
time the model is updated, a `view` function is
used to render the model to HTML.

### The parts

The online interpreter platform comprises several sub-applications:
one for the actual interpreter interface (`Terminal.elm`), one for
loading tutorials (`Book.elm`), and one for interfacing with the
Foogle API (`Foogle.elm`).  Each sub-application lives in its own
Elm-architecture-like loop, with its own model, messages, and
`update` and `view` functions.  Occasionally, in order to have
different sub-applications interact with each other (e.g., clicking on
the `➦` arrow in a code block copies the code from the tutorials app
and pastes it in the interpreter app), messages from one
sub-application are passed to another sub-application; in this way,
the various sub-applications are tied together by the main, root-level
application module (`App.elm`).

In addition to the sub-applications, a separate collection of modules
(prefixed `Factor.*`) handle the definitions (`Lang.elm`), parsing
(`Parser.elm`), evaluation (`Runtime.elm`), and rendering (`Show.elm`)
of Factor code and are used by the interpreter interface to actually
run Factor code.  Since the Factor modules don't, strictly speaking,
constitute an "app", they aren't subjected to the
model-view-message-update Elm architecture.  However, they are
organized similarly:
- `Lang.elm` contains data type definitions amounting to Factor
  objects and syntax trees;
- `Runtime.elm` defines a Factor runtime model (type `State`, which
  stores information such as the current stack contents and a word
  definition lookup table) and implements how evaluating a piece of
  Factor code "updates" the state (function `evalWords`);
- `Show.elm` defines how different Factor values (numbers, strings,
  quotations) are rendered to text/HTML output.
