# ElmConf 2016 Summary

An overview of what happened during ElmConf 2016.  Feel free to submit a Pull
Request!  I'd love to gather all of the slides here at a minimum.

_Also, [Jessica Kerr took some sweet
notes](https://www.dropbox.com/sh/te0s1mysmo248ir/AAA-Fv4DAsd1oDbNW9GKexrUa?dl=0)._

## Photos and Tweets

- Tweets should be under [the #elmconf hashtag](https://twitter.com/hashtag/elmconf?src=hash).
- Here are [just the tweet photos](https://twitter.com/hashtag/elmconf?f=images&vertical=default&src=hash).
- Is there a photo gallery being gathered somewhere?

## Talks

- [Code is the Easy Part - Evan Czaplicki](#code-is-the-easy-part---evan-czaplicki)
- [Beyond Hello World and Todo Lists - Ossi Hanhinen](#beyond-hello-world-and-todo-lists---ossi-hanhinen)
- [Compilers as Therapists, or Why Elm is Good for ADHD - Luke Westby](#compilers-as-therapists---luke-westby)
- [Lightning Talks](#lightning-talks)
- [Building an Interactive Storytelling Framework in Elm - Jeff Schomay](#tktktk)
- [The Clockwork Gardener: Growing an Elm App With Templates - Jessica Kerr](#tktktk)
- [Making Impossible States Impossible - Richard Feldman](#tktktk)

### Code is the Easy Part - Evan Czaplicki

- [@czaplic](http://twitter.com/czaplic)

> Just do a good job. -- Guido van Rossum

#### Random notable bits

##### Batching Work

RE: his projects' tendency to gather a ton of issues before any work is batched:

> At this point people are crying in the streets, they've been displaced by
> floods...locusts...

##### Collaboration

Talked about elm-format, elm-webgl, elm-css, elm-reactor and how people just
build things because they need them :)  Talked about just getting into the
elm-dev, introducing yourself, talking, getting feedback.  The point is: Code is
the easy part!

#### Elm Reactor

Showed off a pretty awesome implementation of elm-reactor / debug mode.  Really
exciting stuff - all history of `Msg`s, see the whole model with expandos.

ZOMG export the `Msg` history, move to different browser, import it there!

#### What is next for Elm?

- Work on code swapping
- Server-side rendering
- Improve Packaging
- ???
  - Platforms?
    - Server
    - Mobile
    - WebAssembly
  - Tools?
    - Type Suggestions
    - REPL
  - Misc?
    - Single-Page Apps Documentation
    - HTTP in core
    - Timely Fixes

One of the points is, what's a way we can work on something that's not just an
improvement but maybe hasn't been done in programming languages before.

#### Slides:

- ?

### Beyond Hello World and Todo Lists - Ossi Hanhinen

- [@ohanhi](http://twitter.com/ohanhi)
- [Here are some notes from Jessica Kerr that I found on Twitter.](https://www.dropbox.com/s/2wesfpakeanhh5z/Ossi%20Hanhinan.pdf?dl=0)

- Overview
- Lessons learned
  - Project structure
  - Refactoring
  - Message passing
  - Code reuse
- Protips (TM)
- War^H^H^HTrue Stories

#### True Story #1

> Generalization: wait for features to stabilize.  Did it too early, had to redo
> it multiple times - just be lazy and duplicate initially!

#### Words

- triplet
  - `(init, update, view)`
  - init returns `(Model, Cmd Msg)`
  - update -> `(Model, Cmd Msg)`
  - view - `Html Msg`
- helper function, returns whatever type
- "Routing" "pages" in SPA with URLs

#### Protip #1

elm-format rocks!

#### Projects

(codenames)

- Ferrari
  - Developed during 2015, thing he wrote "how elm made our work better" about.
  - Tool for newspaper editors to set how the news shows up on the site.  Choose
    articles and article blocks, drag and droppy thing to layout the news site.
  - Started with 0.15
  - probably up to 0.17 now?
  - SSO that only worked on the customer premises
- D25
  - Simple social media thing of sorts.
  - Using sporto/hop for routing and navigation, works really nicely for them.

#### True Story #2

> Everything is **not** a component.

#### Context-Pattern

Have a context you want to use for different modules.  Gives you a simple way to
communicate things down from ancestors in your hierarchy.

```elm
type alias Context =
    { hostname : String
    , user : User
    -- ...
    }
```

```elm
-- Page.elm
init : Context -> (Model, Cmd Msg)
```

#### True Story #3

> Real data can break your app.

In the Ferrari application, when first trying it out with real world data, the
app wouldn't start at all.  Errors in the console even though the code was
compiling perfectly.  Dumbfounded: "how can the data itself break our
application?"

Ultimately had something to do with integers.  Database IDs were more than what
a 32-bit integer can hold!  Circumvented the problem by using a Float type for
it.

#### Authentication

Externalize it if possible.

Your goals:

- anonymous use
- same app logged in/out?
- routes differe?

#### Protip #2

```elm
type RemoteData e a
    = NotAskes
    | Loading
    | Success a
    | Failure e
```

#### Login

```elm
type ToplevelRouter (Maybe Context) (Maybe AppRouter)
-- AppRouter is the routes for the logged in application
```

- When page is loaded, status check to the backend.
- Backend responds either with the logged in user data or 'no logged in user with this session id'.
- If the user exists, initialize the context (which has the user as a plain value).
- Then initialize the `AppRouter` with an actual context value.
  - `AppRouter` never has to deal with `Maybe` - always have a context, always
    have a user.

#### Protip extravaganza!

```elm
case Debug.log "sweet" pin of
    Luke ->
        -- ...
```

```elm
task
    |> Task.map (log "great success")
    |> Task.mapError (log "here's where it fails")
```

```elm
    thing
        |> squeeze
        |> log "squeezed"
        |> tweeze
        |> log "tweezed"
```

#### Project Structure

##### Ferrari

- elm 0.15, no StartApp
- initally had `Model/Article.elm`, `Update/Article.elm`, `View/Article.elm`.
  Eventually moved each model and update into the same file for each type, keep
  view dir.
- The initial move made it impossible to work because all the files were called
  `Article.elm` !!!

##### True Story #4

In the beginning of the project, they wrote unit tests for every function.
Found it wasn't that useful because the majority of functions were just
returning HTML and the unit tests were not terribly useful/helpful.  Toned down
a lot of what they unit tested, focused on the more difficult logical parts of
the code.  Found end-to-end testing was really what they wanted.

> Tests: ROI

##### d25

- elm 0.16, with StartApp
- Initially just put all the files in one folder.
  - Worked for quite a while, til around 20 modules or so it started to get
    harder to find what you were looking for.
- Later, broke up top level dirs:
  - Routers
  - Types
  - Pages
  - Helpers
  - Components

##### Protip #n

> Ports are your friends!

If you're on a schedule, think about...when you're facing a difficult problem in
Elm, and you know that it's easy to work around that using JavaScript, then just
go for it.

##### Personal Learnings

- Reuse
  - everything is not a component.
  - sometimes the tool that you need is maybe just to refactor the `let` block
    into separate functions, or refactor the nested `view` functions into more
    detailed helper functions that you can use from elsewhere.
  - functions are nice
  - state management is not nice
- ROI
  - "How am I going to make it work now?"
    - Very likely in an agile project that it's going to change anyway.
    - Makes sense to not go for the most elegant solution every time, esp. if
      it's going to take a lot of time.
- Structuring
  - Slap everything in the same directory and be done with it until it gets too
    much.  Then refactor to what that current project seems to need.
- Solve the problem **at hand**
  - This is the whole key thing he wanted to stress here.  "I don't want to
    componentize things anymore, I don't want to generalize beforehand.  I just
    want to solve the current problem."
  - That will make it possible for me to move on, and in retrospect I can see
    the whole thing much clearer.

### Compilers as Therapists - Luke Westby

- [@luke\_dot\_js](http://twitter.com/luke_dot_js)

#### 4 Things

- A disclaimer
- An analogy
- A story
- A call to action

#### Disclaimer

> I am not a psychologist.

This talk is 100% personal.

#### Analogy

Poor eyesight and ADHD.

##### Poor Vision

Symptoms:

- Eye strain
- Fatigue or headache after you do a close-up task such as reading
- Trouble focusing on nearby objects
- Blurred vision

When you have poor vision, you miss out on details of the physical world.

##### ADHD

Symptoms:

- Mental strain
- Fatigue or headache after you do a concentration-intensive task such as
  reading
- Trouble focusing on current activity
- Blurred mental perception

> Imagine what it would be like to have blurry vision, but then abstract it and
> apply it to your mind - that's how I experience the world.

You miss out on details of the mental and emotional world.


##### Solutions

To help with poor eyesight, we use a lense to focus light.

To help with ADHD, we can use tools to focus our thoughts.

#### STORY TIME

Wanted to build this cool project, made a bunch of technical decisions up front,
got distracted by them, built some GraphQL stuff, Falcor stuff, open sourced
some things, found all kinds of weirdo issues, found a bug in electron and sent
a PR, etc. etc....but didn't build the project!

Around the same time...

#### THEN I DISCOVERED ELM

You don't have to decide all the things that people talk about with "javascript
fatigue", you just get started and do stuff.  The only thing you have to do with
Elm is build the thing, and the only problem you have to solve is the one you're
interested in.

> Elm has been a tool that changes the way that I think about UI, so that I can
> think about the thing I intend to do.

Built a bunch of open source libraries, 2 running apps he uses every day or
sometimes, gets to talk to people on stage, and from that confidence he was able
to go start his own company.

##### CALL TO ACTION

- For those with similar stories: Share them if you can.
- For those who build things: Listen to those stories.

If we target a11y specifically, we'll find that the experience is just nicer for
everyone.

### Lightning Talks

#### elm-style-animation

[Description from elm-conf site](https://github.com/elm-conf-us/2016.elm-conf.us/blob/master/content/speaker/matthew-griffith.md)

Matthew Griffith showing off the new API for [elm-style-animation](https://github.com/mdgriffith/ui-animation).

- [Talk slides](https://github.com/mdgriffith/elm-animation-talk) - here's the
  repo for his slides :)

#### Functional Data Structures = Tessa Kelly

- [Slides](http://slides.com/tessak/functional-data-structures-3)
  - OK these are pretty cool, live slides you can follow along with from
    slides.com :)
- This was a really cool comparison of a few different ways to build a Binary
  Tree API:
  - Array Based
    - Use Array as our base
    - Derive child indices
    - Hope for a complete binary tree
  - Dict Based
    - Use Dict as our base
    - Derive child indices
    - Continue using helpers to our advantage
    - (kinda cheating since Dicts are...trees)
  - Tree Based, #1
    - Use "pointers"
    - Pretend records have antirely analogous uses to JavaScript Objects
    - Hope that the types stay nice
  - Tree Based, #2
    - Create types to describe the shape of a node
    - Use casing to our advantage

#### I love Elm - Abadi Kurniawan

[Description from elm conf site](https://github.com/elm-conf-us/2016.elm-conf.us/blob/master/content/speaker/abadi-kurniawan.md)

[elm-horizon library](http://github.com/abadi199/elm-horizon)

@engage software
@abadikurniawan

I'm happy when I write Elm.  I'm sad when I write other things.

- Evan clarified we probably won't have elm on the server any time soon.
- What if we just ditched the backend altogether?
- What if I just used Horizon as the backend?  Then I can be happy all the time.
  I write Elm all the time.

##### Why Horizon?

> [Horizon](http://horizon.io/) is a realtime, open source backend for JavaScript Apps.

APIs:

- Authentication
- Collection
- other things?

##### Collection API

List of documents, backed by a table, provides a way to work on that collection.
He grouped this API into `[Write, Read, Modifiers]` sections.

### Building an Interactive Storytelling Framework in Elm - Jeff Schomay

[Description from elm conf site](https://github.com/elm-conf-us/2016.elm-conf.us/blob/master/content/speaker/jeff-schomay.md)

- Built an interactive storytelling tool so he could tell his stories.
- Wanted to avoid the mechanics of spatial navigation or clunky mechanics that
  don't work well for a text-based story.  Wanted it to be simple to build.

> How can I make it as *simple* as possible to tell stories that are as
> *engaging* as possible?

- Just want the client to say "here's the story world, here's how it works" and
  have his project just take it from there.
- Wants to be an html platform, show on mobile, so that works.
- Types and functional nature of elm should make it nice for him to write the
  framework.
- Let the author focus on the story and not the code.
  - We're going to need a good API
- Sharing the evolution of the API

#### The evolution

##### Take 1 - The state machine approach

Transitions that help the story progress.

##### Take 2 - Declarative rule sets

- Just data.  Nice!
- Fairly simple to handle
- But I can't ensure that you've written a rule for every case :( :(
- Story rules are very dense, type-heavy, lots of constructors

##### Take 3 - Adding functions to make a DSL

Just using functions as glues to put together these type constructors.

Ended up accidentally just defining `curry`, `<|` which was nice.

##### Final design

Really nice to write, but had some bad things.  Like you could put a character
in your inventory :( :(

> Make it impossible to do bad stuff.

But doing this led to a less pleasant API, had to do more stuff to add types new
story element types.

[jschomay/elm-narrative-engine](http://package.elm-lang.org/packages/jschomay/elm-narrative-engine/latest)
[jschomay/elm-interactive-story-starter](http://github.com/jschomay/elm-interactive-story-starter)
[@jschomay](http://twitter.com/jschomay)

### The Clockwork Gardener: Growing an Elm App With Templates - Jessica Kerr

[Description from elm conf site](https://github.com/elm-conf-us/2016.elm-conf.us/blob/master/content/speaker/jessica-kerr.md)

(NOTE: I didn't get enough of this taken down as notes because it was kind of
awesome and I was busy watching sorry!)

Elm is awesome to read but sometimes it's a pain to write because of all the
boilerplate to wire things in.

We can fix this!  Templates!

Atomist has a tool for this.  (she then just sent a slackbot a command and got a
github repo out of it with a small Elm app in it)

Adding a text field is annoying - like 5 steps!  She used atomist to have it
modify her code with Pull Requests.

### Making Impossible States Impossible - Richard Feldman

elm-css - turns out you could build invalid stylesheets.  We'd rather not have
this.

(at-rules - charset must be first, imports have to come before namespaces,
namespaces have to come before declarations)

- First thought - validate and sort, done.
- Talked with Evan, he said "yeah what if you made it so people couldn't
  generate invalid stylesheets?"

> Testing is good.  Impossible is better.

> A clearer data model can lead to a clearer API.

- Tracked these different types of things differently in a record.

OK, so what if we aren't doing elm-css stuff?

> Can we just make impossible model states impossible to represent?

#### Survey App

Prompts and responses.

History / Navigation.

Talked about how you might want to move through your questions list, various
ways that you could represent this and landed on something like:

```elm
type alias History =
    { previous : List Question
    , current : Question
    , next : List Question
    }
```

This is a ZipList so you can't have an empty list of questions and you can't
have the current question fail to be one of the questions in this list (since
it's part of the list).

> Can we make it so that depending on certain implementation details is
> impossible?

Yes!  **single-constructor union type**

```elm
type History =
    History
        { previous : List Question
        , current : Question
        , next : List Question
        }
```

Noted that you could pattern match on the value in the function arguments since
it just has a single constructor.

Now let's add a "Status Bar" to show action results and whatnot.  i.e. "Question
Created", "Question Deleted". (oh yeah and maybe we want to undo when question
deleted)

```elm
type Status
    = NoStatus
    | TextStatus String
    | DeletedStatus String Question
```
