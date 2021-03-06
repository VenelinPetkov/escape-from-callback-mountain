# Escape from Callback Mountain!

[![Build Status](https://travis-ci.org/justsml/escape-from-callback-mountain.svg?branch=master)](https://travis-ci.org/justsml/escape-from-callback-mountain)

## Refactoring NodeJS/JavaScript - a 2017 Guide

I am a big fan of Functional Programming and Modular JavaScript. This project's goal is to demonstrate the latest Functional Promise patterns, while taking you through a refactor of real world callback-based NodeJS/JavaScript. 

The overall [technique I demonstrate](#after) is what I call the **'Functional River' pattern**. Where your input/parameters/data is the water, and the code forms the riverbed. More or less. 
It is an async version of the [Collection Pipeline](https://martinfowler.com/articles/collection-pipeline/) pattern.

To the Haskell pros out there, before you flame me for not defining 'monad', this is meant to be a more welcoming place.
_So forgive me if I skip the overblown theory & jargon._

### 'Functional River' Goals/Benefits:

* Higher level logic implemented with **multiple smaller single-purpose functions, assembled to read like a story.**
* Reduce bugs by eliminating ad hoc logic. (e.g. one-off transformations, untested validation)
* Use same interface for both synchronous & asynchronous code. (`promise.then(value => alert(value))`)
* Prefer immutable, stateless code as essential building blocks.
* Less elaborate, modular code is naturally more reusable.
* Easier to move logic around - rebundle simple functions as needed to create new higher-order functions.
* Increased testability - eliminate hidden logic.
* Substantially faster code readability - versus [methods which muddles the important parts, and further hides ad hoc error/glue code](https://github.com/justsml/escape-from-callback-mountain/wiki/Beating-a-dead-horse%3F).

> Note: Relies on ideas from Lisp to SmallTalk - adapted to a JavaScript world.
> Additionally, I happen to use [Bluebird Promises](http://bluebirdjs.com/docs/features.html). Apologies to `Promise Resistance Leader` [Brian Leroux](https://twitter.com/brianleroux). For alternative patterns please read my more detailed article demonstrating [4 Functional JavaScript Techniques (with Examples)](http://www.danlevy.net/2017/03/10/functional-javascript-composition/)

#### Have feedback, fixes or questions? Please create issues or PRs. Or just complain at me on [twitter @justsml](https://twitter.com/justsml).

If you feel this subject has been thoroughly explored, please see my post [Beating a dead horse?](https://github.com/justsml/escape-from-callback-mountain/wiki/Beating-a-dead-horse%3F)

----------

## Example Task:
### Authentication Method

Here's a rough visualization of our function:
![image](https://cloud.githubusercontent.com/assets/397632/26270285/fecd78ca-3cb6-11e7-9a3f-fbe327cec18b.png)

## See both [Before](#before) and [After](#after) examples below.

## Before
### Node-style Callbacks w/ Nesting

> **Note:** This is intentionally _reasonable_ callback code. Even if nested. Not trying a [straw-man](https://en.wikipedia.org/wiki/Straw_man) attack.

![callback-mountain-before](https://cloud.githubusercontent.com/assets/397632/25775652/5e49b444-3267-11e7-937c-8b786da9314a.png)

## After
### 'Functional River' Pattern
![callback-mountain-after](https://user-images.githubusercontent.com/397632/28086871-b60a25ba-663d-11e7-9190-560556f5619b.png)

## Key Steps

1. [Step 1: Break Up The Big Functions](https://github.com/justsml/escape-from-callback-mountain/wiki/Step-1:-Break-Up-The-Big-Functions) - read the code: [PR #2: Flatten Functions](https://github.com/justsml/escape-from-callback-mountain/pull/2/files?diff=unified)
1. [Step 2: DRYer Code](https://github.com/justsml/escape-from-callback-mountain/wiki/Step-2:-DRYer-Code) - read the code: [PR #3: DRYer Code](https://github.com/justsml/escape-from-callback-mountain/pull/3/files?diff=unified)
1. [Step 3: Cleanup Code](https://github.com/justsml/escape-from-callback-mountain/wiki/Step-3:-Post-Cleanup) - read the code: [PR #5: Post Cleanup](https://github.com/justsml/escape-from-callback-mountain/pull/5/files?diff=unified)


## Pros & Cons

#### Pros

* Less ad hoc code results in:
  * More uniform code between different teams & developers,
  * Performance tooling & refactoring is an appreciably better experience,
  * More certainty about code correctness,
  * Higher code reuse.
* 100% Unit Testability
  * Unit tests uniquely prove you found, understand, AND resolved a given bug,
  * Faster bug resolution process,
* Flatter code hierarchy == less filler to remember
  * Re-organizing code is easier & less prone to bugs - with [Pure-ish Functions](https://en.wikipedia.org/wiki/Pure_function)


#### Cons

* Performance. I've run some micro-benchmarks - it's not awesome. However, 3 important things:
  1. It's **not meaningfully slower in real world applications.**
  1. If it is necessary, performance analysis & tuning is a much improved experience. Smaller functions make it easier to see where slow code lurks - especially if you profile unit tests.
  1. As more people adopt these patterns, things will improve. V8/Chrome has been impressively fast at optimizing for emerging patterns.
* Debugging can be more difficult. Though I have updated my dev tricks to debug this style of code, even without the confort of Bluebird's error handling. I'll add more sample scripts for this later.
* Something **new to learn**. Deal with it, you're a developer.
* If you have an existing project with lots of code, the unfortunate **reality is: Refactors Suck**.
* EventEmitter- & Stream-based code is not improved much, if at all, using this technique. Look into RxJS
  - Ongoing experiments include simple closures, extend Promise with `EventEmitter`, or using Bluebird's `.bind` to inject variable state into the Promise chain. (I know, "ugh side-effects, gross." PRs welcome.)

This area of Functional JS patterns, and consenus around it's best practices has plenty room to go.

## Concerns

#### Some really smart people out there have pointed out potential problems with over-modularization.
![image](https://cloud.githubusercontent.com/assets/397632/25776158/12d0be56-3274-11e7-87c9-7dee8a5e4b09.png)

While true of most coding patterns, an overly-done flat & modular JS Project can feel more disorganized over time.
Project and code discipline is just as important as it's always been. Also, we're still developing consensus around Functional JS patterns.

Another solution I've found is to add a **Code Style Guide** preferably with naming conventions - [see my thoughts on that subject](http://www.danlevy.net/2015/09/22/beautiful-engineering-models-and-data/).
This becomes much **more important as team size grows**.

When done right, one of `Functional River`'s **greatest strengths** is the ability to **relocate & rearrange** modules with **low risk**. If this still feels risky, your modules are still too entangled.

----------

#### Ultimately my goal is to better understand & advance Modular + Functional JS patterns. Hopefully I can sway some of the skeptics along the way :crossed_fingers:

-----------

### Please Star this project and [vote on HN](https://news.ycombinator.com/item?id=14675068) ❤️

-----------

### Credits & Inspiration

I highly recommend reading (or watching) every single link here.

- Tom Stuart's Purely Amazing [Programming with Nothing](http://codon.com/programming-with-nothing)
- Eric Elliot's Tour de force: [The Two Pillars of JavaScript](https://medium.com/javascript-scene/the-two-pillars-of-javascript-ee6f3281e7f3)
- Ashley Williams Speaks The Modular Tructh in [A Brief History of Modularity | JSConf EU 2017](https://youtu.be/vypCsVm5z28)
- And Eric's [Rise and Fall and Rise of Functional Programming (Composing Software)](https://medium.com/javascript-scene/the-rise-and-fall-and-rise-of-functional-programming-composable-software-c2d91b424c8c) is damn good too ;)
- Martin Fowler expertly lays down the Ruby + Haskell pipeline in [Collection Pipeline](https://martinfowler.com/articles/collection-pipeline/)

