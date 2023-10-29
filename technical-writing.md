# Technical writing

+ > Put both the spelled-out version and the acronym in `boldface`: **My Acronym** (**MA**)
+ > Do **not cycle** back-and-forth between the _acronym_ and the _expanded version_ in the same
  document.
+ > **don't use** _pronouns_ **separated** by more than **5 words** or when **ambiguous**. better to
  repeat the noun
+ > use specific verbs instead of generic ones like `is`, `am`, `are`, `was`...
+ > **don't start** sentences with `There is`, `There are`.
+ > Eliminate or **reduce** _extraneous words_
+ > **Reduce** _subordinate clauses_ (optional)
    + > If the _subordinate clause_ branches off into a **separate** _idea_, consider **dividing**
      the
      offending _subordinate clause_(s) into separate sentences.
+ > Reserve `which` for nonessential subordinate clauses, and use `that` for an essential
  subordinate c (If you read a sentence aloud and **hear** a **pause** just before the subordinate
  clause, then use `which`).
    + > Place a `,` before `which`; do not place a `,` before `that`.
+ Lists and tables
    + > Keep list items parallel. All items in a parallel list match along the following parameters:
      > + grammar
      > + logical category
      > + capitalization
      > + punctuation
    + > Start numbered list items with imperative verbs
    + > Create useful tables
      > + **Label** each column with a meaningful _header_
      > + **Avoid** putting _too much text_ into a table cell.
      > + strive for _parallelism within individual columns
    + > **Introduce** each list and table
      > + we recommend **putting** the word `following`
      > + **Terminate** the introductory sentence with a `colon` rather than a `period`
+ Paragraphs
    + > Write a great opening sentence
    + > Focus each paragraph on a single topic. A paragraph should represent an independent unit of logic.
    + > ruthlessly delete (or move to another paragraph) any sentence that doesn't directly relate
      to
      the current topic.
    + > Readers generally welcome paragraphs containing three to five sentences,
    + > **Answer** `what`, `why`, and `how`:
      > + **What** are you trying to **tell** your reader?
      > + **Why** is it important for the reader to **know** this?
      > + **How** should the reader **use** this _knowledge_? Alternatively, **how** should the
          reader
          **know** your _point_ to be **true**?
+ Audience
    + > In other words, make sure your document provides the information that your audience needs
      but
      doesn't already have:
      `good documentation = knowledge and skills your audience needs to do a task − your audience's current knowledge and skills`
      > + Who is your target audience?
      > + What is your target audience's goal? Why are they reading this document?
      > + What do your readers already know before they read your document?
      > + What should your readers know or be able to do after they read your document?
+ Documents
    + > **State** your document's _scope_
    + > **Summarize** _key points_ at the start
    + > **Compare** and **contrast**

# Example

+ Who is your target audience?
    + > Colleagues from the Committed team.
+ What is your target audience's goal? Why are they reading this document?
    + > To set up a configured CircleCI pipeline in combination with GitHub and AWS without the need
      to refer to _CircleCI_ documentation.
+ What do your readers already know before they read your document?
    + > They are familiar with the _project_. They master the skills of a average junior _DevOps_
      developers. They have heard about _CircleCI_ in general but have never set one up by
      themselves.
      > They know how to use _Spring Boot_, _GitHub_ and _AWS_ on a basic level including the
      following:
      > + Program and test a _Spring Boot_ application.
      > + Obtain secret keys from _GitHub_ and _AWS_.
+ What should your readers know or be able to do after they read your document?
    + > My target audience will be able to set up and use the configured _CircleCI_ _pipeline_
      including the following:
      > + Understand when the pipeline is triggered.
      > + Set up and use the configured _CircleCI_ _pipeline_ including:
          >   + Configure application instances for different environments.
          >
      + Input secret key from _GitHub_ and _AWS_.
      >   + Input credentials for different environment available on _AWS_.