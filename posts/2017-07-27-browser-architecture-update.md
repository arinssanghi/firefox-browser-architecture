
[Link to mailing-list archive](https://groups.google.com/d/msg/firefox-dev/ueRILL2ppac/RxR9lLPkAwAJ)

In the spirit of the Quantum Flow and Photon Newsletters - an update on what the Browser Architecture team is doing:

The Browser Architecture Team is new-ish - it’s existed since the re-org earlier this year, and its goal is to look ahead and make plans for our architecture.

We want to build the world’s best web browsers. Which is easy to say, but much harder to actually do.

mozilla-central is a big complex software project. Its size and complexity slows us down, and it’s daunting to think about making it easier to reason about. We’ve often put off making life easy for ourselves because our users come first. We generally prioritize fixing bugs for users over internal work.

But sometimes you can go faster for users by focusing on going faster yourself. Part of what the Browser Architecture team would like is to tackle some of the hard problems of our own development velocity.

We also need to look at some problems that will take several years to address. Problems that don’t fit into any project area roadmap.

We have a mandate to help build the world’s best web browsers, but how? Our strategy is twofold:
1. Kickstart a streamlined developer experience by removing roadblocks to immediate progress.
2. Consciously plan our software architecture at an organization-wide level.

So what are the specific problems we are looking at?

# Workflow

UI development in mozilla-central somewhat mirrors the web development ecosystem, however one way we’ve fallen behind is in unit tests for frontend code. In mozilla-central we usually rely on integration tests rather than unit tests. Integration tests end up being slower and are more likely to fail randomly. In addition they steal focus and don’t allow for integration with code editors, to see test results as you type. We’d like to figure out how to write more unit tests in our codebase.

We’re looking at turning headless mode into a way to continue work whilst running tests, rather than needing to run a VM or turning to sword-fighting.

We’ve also spent some time looking at ways to make developing the Firefox frontend more like web development, with features like CSS hot reload and refreshing the UI without rebuilding and restarting the process.

In parallel, we’re also looking at ways that we can improve Gecko development efficiency, especially on build times and debugging (Rust, rr, gdb on Mac).

# XUL and XBL

Our user interface language has problems. It’s uncared for, so it’s buggy and sometimes we don’t land fixes that we know about. It’s largely undocumented or worse, documented wrongly, so it’s hard to fix. Also it behaves almost like the web, which can lead web developers to think they understand it better than they really do.

We care about HTML, so that’s documented and optimized. XUL? Not so much.

The first talk I heard about Servo was in 2013. At the Q&A at the end someone asked about XUL and Servo — the answer was "There is no XUL in the product at all". Applause from audience. So the tide has been against XUL for at least 4 years, but so far we’ve not made headway in tackling it. The Browser Architecture team is approaching it from various angles:

Can we remove XUL and XBL piece by piece? We are experimenting with a couple of approaches with the preferences UI. Could we convert the XBL to JS modules and Web Components ‘in place’? Alternatively, how much work is it to rewrite it in HTML, using something like React? Or more dramatically, how far up the stack should our use of Rust go? Maybe we should consider a UI generated by Rust?

Alternatively, what’s the minimum we could do to just fix and document what we’ve got. Perhaps we should just focus on removing XBL? Maybe the plans above are too much work and we need to invest elsewhere entirely?

# User Data Storage and Sync

We don’t have a good story for storage of user data. We wouldn’t sit down to design the profile directory and come up with what we have today, and we need a better story to be able to sync more readily to Firefox for Android and iOS and to adapt to unanticipated and evolving product needs.

Recently we've been exploring Project Mentat, which promised some advances in helping us rapidly evolve data storage, and making syncing of data less troublesome. We decided that porting all of Firefox’s storage to Mentat was too much work right now, but the problem of improving user data storage and sync remains.

So we’re looking at the big picture of storage and sync and how we can share data not just between desktop and mobile, but also to help us develop and test new products in the market.

# A Monolithic Platform

We’re considering what it would look like to turn Gecko inside out so it’s more modular, can be reused in smaller parts and is easier to test, as with Rust crates in Servo. Changing that is hard — really hard — but the potential value is also significant.

We're looking at existing Firefox components and wondering if we can extract them (or rewrite them) as separate components that we could reuse in situations where we can't rely on the Gecko runtime environment, and we’re looking at the work on GeckoView to see how it can help us reuse Gecko outside Firefox.

----

There are a lot of ideas there. Maybe some sound cool, maybe some sound crazy. We’re planning on writing up our findings on each of the areas so look out for future posts where we drill down into some of these ideas.

Thanks

Joe Walker (on behalf of the Browser Architecture Team)

Feedback is welcome. Either reply here or find Nick Alexander, Brendan Dahl, Brian Grinstead, Rob Helmer, Randell Jesup, Myk Melez, Richard Newman, Victor Porof, Emily Toop, Dave Townsend, Peter Van Der Beken, or Joe Walker near some watercooler (like #browser-arch).