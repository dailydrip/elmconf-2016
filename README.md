# ElmConf 2016 Summary

An overview of what happened during ElmConf 2016.  Feel free to submit a Pull
Request!  I'd love to gather all of the slides here at a minimum.

## Photos and Tweets

- Tweets should be under [the #elmconf hashtag](https://twitter.com/hashtag/elmconf?src=hash).
- Here are [just the tweet photos](https://twitter.com/hashtag/elmconf?f=images&vertical=default&src=hash).
- Is there a photo gallery being gathered somewhere?

## Talks

- [Code is the Easy Part - Evan Czaplicki](#code-is-the-easy-part---evan-czaplicki)
- [Beyond Hello World and Todo Lists - Ossi Hanhinen](#beyond-hello-world-and-todo-lists---ossi-hanhinen)

### Code is the Easy Part - Evan Czaplicki

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
