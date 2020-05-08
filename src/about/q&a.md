# Questions and Answers

We gave a presentation on our work around when we complied these pages. This presentation is not currently available, but we are including below the
questions we were asked, as well as our responses to them.

## What would you say are the top 5 barriers to entry for developers new to Factor?

We were all beginning developers in Factor a little over a year ago, and what follows are the top 5 barriers we faced when learning the language. A lot of them persisted despite the excellent instruction and advice from John that we had at our disposal.
1. One barrier is Factor’s programming paradigm. Stack based languages have syntax that is "flipped" compared to most other languages ("if a then b else c" in other languages becomes "a b c if"). "Elegant code" in other languages might not be the same as "elegant code" in Factor and certain useful Factor/functional/stack-specific concepts like combinators (e.g. bi, tri, etc.) have no analogues in other popular languages. By way of analogy, if Java is like Spanish, then C# is like Portugese and Factor is like Japanese. On top of surface-level syntactic differences (Japanese is subject object verb order as opposed to the romance languages’ tendency to be subject verb object), there also are deeper cultural differences on how to phrase things.
2. One other barrier is the syntax. While not a major barrier to understanding Factor, as beginners we found it frustrating at first. The creator of Elm blogged about how this so-called "syntax cliff" prevents beginners from adopting new languages, despite having no conceptual significance. For an example of difficulties we faced: in other languages, "special" syntax (e.g. `[]` for lists, `;` to terminate statements, etc.) is not whitespace sensitive. For instance, in Python, `[1, 2, 3]` and `[ 1, 2, 3 ]` are equally valid. Coming from these languages, Factor's mandatory spaces between "special tokens" (technically, they're parsing words, which is why they require spaces, but that's a nuanced concept that newcomers are unlikely to know about) can be confusing at first. Some similar examples:
    - Mandatory spaces in arrays and quotations: `{ 1 2 3 }` `[ print ]`, not `{1 2 3}` or `[print]`
    - Mandatory spaces before ending semicolons in word definitions: `: add ( -- ) + ;`, not `: add ( -- ) +;`
    - Mandatory semicolons at the end of word definitions and `USING:` declarations, but forbidden semicolons after `IN:` statements. Coming from a language like Java or C, where semicolons are used on every line to end a statement, this inconsistency is jarring at first (again, parsing words—but beginners don't think of it that way!).
3. Another barrier is documentation. Factor documentation isn’t really lacking, but we found it difficult to navigate at first, especially when we don’t know the name of the wanted word/vocabulary beforehand. This is part of the reason that we included Foogle into our online platform.
4. Another barrier is online presence. Though we had the privilege of consulting with John, it may be somewhat difficult for other learners to find a community of Factor programmers to discuss the language or learn more about it. There are communities on IRC and Reddit, but neither are very active. It is not very easy to quickly have questions about Factor answered on a place like StackOverflow.
5. Finally, tutorials. There are also fewer tutorials that exist for Factor than for other, more popular programming languages. Quite a few tutorials do exist, you just need to find them. On our online platform we compiled a list of factor tutorials and resources in order to help Factor programmers find new resources.

## What other non-trivial contributions to Factor did you think of? Why did you choose these 3? If you had infinite energy/time/skill, which one(s) would you choose to work on?

We actually originally started another project, which was compiling Factor to LLVM. However, we realized that this project wasn’t feasible in the amount of time we had, and switched to developing the online platform and Foogle. Other ideas we proposed included a more strict type system, package management (so that all Factor vocabularies aren’t in the same place), and adding native integers to Factor. We picked the projects we did because we were most interested in them, and because we felt that we could complete them within the constraints of the clinic program.

With infinite energy/time/skill, we think the obvious answer would be “all of them” :). But supposing we had to pick, we would like to put more of a focus into putting Factor “on the map.” Like continuing to make tutorials, useful real-world libraries, or promoting it via cool showcase projects.. We feel that if we put enough effort into growing the community, the community can then grow itself and Factor with it. To us, this seems like the most bang for your buck.

## During the presentation, and during the questions after the presentation, they talked about the relationship between the stack based nature of Factor and the performance. I don’t think it’s as simple as what they explained. 

That’s true! However, we had a time limit and wanted to convey just enough information about Factor so that our projects made sense to the audience.

## What do you see are the advantages of stack-based languages? (I don’t think it’s performance)

One advantage often cited by stack-based language proponents is that thinking in terms of a "stack" and operations on the stack naturally encourage a pipeline-like, "forward"-composition way of programming.

An example scenario to illustrate: say we want to fetch a list of elements from a web API, process the elements somehow, and add them to a local data store. In an imperative language like Python, the code might look like this:

```python
for element in fetch_elements_from_api():
  add_to_local_store(process(element))
```

Notice how the code, reading left-to-right, reverses the `add_to_local_store` and `process` steps logically: even though we process data first and then add it to the local store, the syntax forces us to write the last step first and the first step last. (You can thank the mathematicians for this function application syntax.) The corresponding Factor code might look like this: 

```forth
fetch_elements_from_api [ process add_to_local_store ] each
```

Observe that the code directly parallels, left to right, the composition of steps: "fetch data from the API", then "process and then store" each fetched element.

## How would you compare the first program tutorial approach to a cookbook style approach to learning how to accomplish things in Factor?

I think both approaches offer different benefits when learning Factor. A cookbook style approach gives programmers a set of tools that they can use when programming. These tools are important since they can be used over and over when programming. The first program tutorial approach is a great introduction to Factor. It allows a new Factor programmer to jump right into Factor and start coding quickly. This style of tutorial teaches users how to combine ideas and tools that they have learned in order to create larger programs. I think utilizing a combination of both tutorials when learning Factor would be the most useful approach.

## They didn’t mention the prebuilt binaries (development and stable), which have lower barrier to entry than building from source.

That’s definitely true. However, it still takes a fair amount of time to download, which was more of the point. Additionally, downloading a binary is still a much larger barrier to entry than simply visiting a website. The point is, even if downloading a binary is not that much of a barrier, to some people simply the idea of having to download & install something could make them lose interest, especially since they probably haven't heard much of Factor before; providing them with a completely no-barriers way to experiment with Factor online may make it easier to expose new users to it.

## What are the differences, advantages and disadvantages of their interpreter vs [fjsc](https://docs.factorcode.org/content/vocab-fjsc.html)?

Whoa! We didn't know fjsc existed. One key difference in philosophy, based on taking a quick glance at the fjsc documentation, is that our interpreter aims to replicate the interactive Factor "experience" in the browser, not just the execution behavior of Factor as a programming language. This means having clickable/introspectable values, user-friendly error reporting, interactive documentation, etc.--not just "running Factor code in a browser." Of course, we're not there yet and are missing many of these features--but one example of something we do have right now is being able to "click" on a piece of Factor code in our tutorials and have it pasted into the Factor listener (something similar exists on the desktop Factor listener); this is something that fjsc doesn't try to do at all. In terms of language (syntax/vocabulary/feature) coverage itself, fjsc is probably ahead of our interpreter--but our current implementation might be easier to adapt to our user-friendliness/interactivity goals later on.

## Did you compare assembly generated by factor and C for any of the tensors code to see where performance differences were?

Unfortunately, we did not have time to do this before the end of the year. Looking at the assembly directly might have given us some good insight into where the differences lie. This is something we will suggest for future developers looking to build on top of our vocabulary.
