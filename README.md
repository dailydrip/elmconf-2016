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

Generalization: wait for features to stabilize.  Did it too early, had to redo
it multiple times - just be lazy and duplicate initially!
