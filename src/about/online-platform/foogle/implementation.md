## Implementation

There are three components to Foogle's implementation.
1. Database generator
1. Search engine
1. Frontend

First, we generate a database containing input-output information for
all of the functions in Factor. This database is read by the search
engine. The frontend is used to send queries to the search engine,
which returns them in a format that the frontend then displays to the
user.

### Database generator
The database generator is a simple script written in Factor that
iterates over each function and creates a JSON object documenting
it. The JSON object contains information such as the function’s name,
a link to its help page, and its stack effect (which is what describes
its inputs and outputs). It creates a database of all these JSON
objects.

### Search engine
The search engine is a webserver written in Haskell using
[Servant](https://www.servant.dev/). It first parses the database
information (it has to do nontrivial parsing for the stack effects,
which are serialized as Strings instead of JSON objects). When it
receives a query, it searches the stack effect information for each
function to find the functions closest to the query.

### Frontend
The frontend is a simple webpage written in Elm as part of the overall
Factor Online Platform. It has rudimentary support for querying and
rendering the server’s responses.
