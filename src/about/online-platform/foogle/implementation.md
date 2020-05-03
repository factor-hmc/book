## Implementation
There are three components to Foogle’s implementation and a fourth auxiliary interface.
1. Database generator
2. Search engine
3. Frontend
4. Command-line application

First, we generate a database containing input-output information for all of the functions in Factor. This database is read by the search engine. The frontend is used to send queries to the search engine, which returns them in a format that the frontend then displays to the user.

### Database generator
You can find the database generator generator at [foogle/make-database.factor](https://github.com/factor-hmc/foogle/blob/master/make-database.factor).

The database generator is a simple script written in Factor that iterates over each function and creates a JSON object documenting it. These objects are described below.

#### Function objects

| Key              | Value Type        | Value Description                                                                       |
|------------------|-------------------|-----------------------------------------------------------------------------------------|
| "name"           | String            | Name of the function.                                                                   |
| "effect"         | Object or Boolean | If the function has a stack effect, an object representing it. Otherwise, the value `false`. |
| "url"            | String            | URL to the function's help page on the Factor documentation.                            |
| "vocabulary"     | String            | Name of the function's vocabulary (library).                                            |
| "vocabulary_url" | String            | URL to the vocabulary's help page on the Factor documentation.                          |

#### Stack effect objects

| Key                   | Value Type        | Value Description                                                                                                                                                                                                                             |
|-----------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| "in"                  | Array             | List of input stack effect variables                                                                                                                                                                                                                |
| "out"                 | Array             | List of output stack effect variables                                                                                                                                                                                                               |
| "in_var"              | String or Boolean | If the function has an input row variable, its name. Otherwise, the value `false`.                                                                                                                                                            |
| "out_var"             | String or Boolean | If the function has an output row variable, its name. Otherwise, the value `false`.                                                                                                                                                           |
| "terminated?"         | Boolean           | Does the function terminate in an error? Factor functions that do this usually have their output stack effect declared as `*`. See [this function](https://docs.factorcode.org/content/word-decode-error%2Cio.encodings.html) for an example. |
| "effect_descriptions" | Object            | An object that has keys equal to the names of the stack effect variables and values equal to their description on the function's help page. It may not have all stack effect variables in it (in fact, it may have none).                                                                                                   |

#### Effect variables
There are three possibilities for an effect variable’s value.

| Variable Type                   | Value Type | Value Description                                                                                                                              |
|---------------------------------|------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| Stack Effect Variable           | String     | A string describing the name of the variable.                                                                                                  |
| Typed Stack Effect Variable     | Array      | An array of two strings. The first is the name of the variable and the second is the type of the variable.                                     |
| Quotation Stack Effect Variable | Array      | An array of a string and an object. The string is the name of the variable. The object is a stack effect object representing its stack effect. |

The generator creates a database of all functions that are in Factor when it is loaded (those obtained from the function `all-words`). This should be all of the functions in the official Factor ecosystem.

This database is an array of function objects. It is not too difficult to parse the database, but be aware that many values are heterogeneous (i.e. they can have more than one possible type). “in_var” for effect objects, for example, may be strings or the boolean value `false`.
### Search engine

#### Overview

The search engine is a webserver written in Haskell using [Servant](https://www.servant.dev/). It first parses the database information. When it receives a query, it first parses the query into a stack effect, then searches functions to find those with stack effect closest to the query.

Parsing the database is not difficult, and follows the specification of the JSON values.

The query parsing closely mimicks Factor's own parser for stack effects. Tokens, for example, must be space-separated. A general description of the form of stack effects can be found [at this webpage](https://docs.factorcode.org/content/article-effects.html). There are a few differences for Foogle's parser. First, it is more permissive in what types of dashes it accepts as a separator between in variables and out variables (it will accept unicode em dashes, as searching on a phone can generate those by accident). Second, it allows users to search by types as if they were writing a stack effect declaration for a [TYPED: function](https://docs.factorcode.org/content/word-TYPED__colon__,typed.html). This syntax is of the form `effVarName: type`. It also allows users to specify _multiple_ types for a given variable by separating the type with a bar `|`. These are presently treated as a disjunction (i.e. only one of the types needs to match). Finally, it does not properly figure out whether an effect has the property `terminated?`. It's very likely that adding this property just requires that you check if the last effect variable in the output effect is called `*`, but this wasn't confirmed.

An example query showcasing most of the differing features of the parser is shown below. Comments are rendered after `!`, but this isn't valid syntax for the parser (although it could be extended to allow it).

```
( var1: boolean            ! declare types like in the TYPED: effect
  var2: string | sequence  ! declare disjunction types
  —                        ! em-dash instead of --
  *                        ! parsed as a normal stack effect, when it probably should change the terminated? property
)
```

Foogle conducts its searching using the metric of edit distance from one stack effect to another, where insertions and deletions are disincentivized. Effect variables with matching types have the most incentive to being matched. If the types cannot be matched, the engine attempts to match the names of the variables.

#### Code description

You can find the server code at [foogle/server](https://github.com/factor-hmc/foogle/tree/master/server). A summary of each file is provided below.

[foogle/server/Main.hs](https://github.com/factor-hmc/foogle/blob/master/server/Main.hs) compiles to the server executable. It loads the database and passes it to a function that makes and runs the server.

[foogle/server/Server.hs](https://github.com/factor-hmc/foogle/blob/master/server/Server.hs) is the actual server code. The type of the API (`QueryAPI`) specifies the inputs the server accepts and the outputs it returns. The `server` function implements the logic that validates inputs, calls a function to search the database, and returns an array of JSON objects (`FoogleResult`s). It uses [Servant](https://www.servant.dev/).

The main Foogle and searching logic is in
[foogle/src](https://github.com/factor-hmc/foogle/tree/master/src).

[foogle/src/Types.hs](https://github.com/factor-hmc/foogle/blob/master/src/Types.hs) contains the definitions of most types and their implemented type classes used by the search engine. Relevant type classes include the `Show` instances, which describe how to render the various data types, and the `FromJSON` instances, which describe how to parse the types from JSON. The JSON parsing uses [Aeson](https://hackage.haskell.org/package/aeson).

[foogle/src/EditDistance.hs](https://github.com/factor-hmc/foogle/blob/master/src/EditDistance.hs) is a (mostly copied) implementation of a dynamic-programming based edit distance algorithm.

[foogle/src/Parse.hs](https://github.com/factor-hmc/foogle/blob/master/src/Parse.hs) is a parser for stack effects. It supports some custom features that Factor doesn’t, like parsing multiple types for a stack effect variable as well as using unicode dashes instead of `--` to separate inputs and outputs (this is to help people searching on devices that make `--` into these unicode dashes). It uses [megaparsec](https://hackage.haskell.org/package/megaparsec) for parsing.

[foogle/src/Infer.hs](https://github.com/factor-hmc/foogle/blob/master/src/Infer.hs) is where the search engine guesses types for the stack effect variables. This is done by inspecting their names and documentation.

[foogle/src/Search.hs](https://github.com/factor-hmc/foogle/blob/master/src/Search.hs) is where the searching happens. Most of the code in this file is describing the various costs incurred or removed from matching of effect variables. The search itself is just taking the first `n` elements after lazily merge sorting by cost (the fact that the sort is lazy is important since it can make fewer passes over the database).

[foogle/src/Argparse.hs](https://github.com/factor-hmc/foogle/blob/master/src/Argparse.hs) is used for the command-line application.

### Frontend
The frontend is a simple webpage written in Elm as part of the overall Factor Online Platform. It has rudimentary support for querying and rendering the server’s responses. It allows users to query by stack effect and specify the number of responses (up to 500, the server rejects requests for more in the interest of avoiding long response times).

You may find this page's source code at [simple-interpreter/src/Foogle.elm](https://github.com/factor-hmc/simple-interpreter/blob/master/src/Foogle.elm).

### Command-line application
Foogle has a command-line application for offline use.

You can find the command-line code at
[foogle/app](https://github.com/factor-hmc/foogle/blob/master/app).

[foogle/app/Main.hs](https://github.com/factor-hmc/foogle/blob/master/app/Main.hs) parses the arguments and then runs them through the search engine, reporting the outputs. This executable dispatches to the same modules that the server uses, as well as `Argparse.hs`.

[foogle/src/Argparse.hs](https://github.com/factor-hmc/foogle/blob/master/src/Argparse.hs) describes how to parse the command line arguments. It uses [optparse-applicative](https://hackage.haskell.org/package/optparse-applicative).
