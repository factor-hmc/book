## Future work
Although Foogle currently is functioning, there is much room for
improvement. Of its three parts, the database generator is the most
fully-featured. The most-desired improvements lie in the search
engine. There are a few open design questions for searching, as well
as potential efficiency issues for larger queries.

### Database generator
The database generator, though a rudimentary script, does successfully
generate information for each function. !!! We are presently working
on extracting the help documentation for the function’s stack effect
(this should already be done by the time this post is released) !!!,
after which we will have generated all the information that we want
thus far. More information can be taken from functions as desired.

### Search engine
The search engine is where the most improvement can happen. Searching
presently is quite brittle, especially to reordering of parameters. It
relies sometimes on matching the names of parameters, especially for
parameters without any type information. For example, `elt` in the
sample query exactly matches the name of the output parameter for
`nth`; if it didn’t, `nth` would appear a little lower in the search
results.

There is also more that can be done in terms of specifying the query:
it would be nice to enable users to choose whether they’d like to
search the names of functions, the stack effects, other features, or
some combination.

The search is also not optimized for efficiency. The query is compared
to every function in Factor and the top few are returned. This is done
by using a lazy merge sort. Presumably, some form of keying based on
stack effect (even by relative size or complexity) would allow us to
narrow down the results and make more performant queries. Searching at
least isn’t on the order of minutes, but it is far from instant,
sometimes taking a few seconds. This isn’t a problem now, when there
are only a few users, but may be an issue in the future.

### Frontend
There is also a lot that can be done for the frontend in terms of
usability. Right now users have to search by typing in a well-formed
stack effect query. It would be nice if they could also use a
graphical interface to specify what the inputs and outputs are, as
well as their types. The frontend would also benefit from some general
beautification. It is not as important to improve the frontend as it
is the search engine, as all of the business logic is in the search
engine, but it would give a nice layer of polish.
