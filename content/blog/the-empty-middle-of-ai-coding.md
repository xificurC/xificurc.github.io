+++
title = "The Empty Middle of AI Coding"
date = 2026-04-01
+++

The AI coding user base is concentrated at the 2 extremes:

- on one end we have the skeptics - "the AI bubble will burst", "AI doesn't improve coding in any way".
- on the other end - the vibe coders - "I one-shot a browser", "AI automated 80% of my work".

With such strong polarization one starts thinking, who's right?

## I traveled there and back and landed in the middle

I was a strong skeptic. I tried AI coding every 6 months, confirming my skepticism - it's not working. Come January 2026 - Opus 4.5 was getting traction. My boss asked me to try it again. I entered for the ride:

- my initial attempts were disappointing again: bad code, failing to understand the problem, hallucinating - the typical issues. I could see the LLM has improved but it still seemed largely useless.
- 2 weeks later I got my hands on some very easy, repetitive work: I thought hey, maybe Claude can code this up for me. And it did, almost perfectly. I started using it for mundane, well defined tasks.
- through sustained usage and the release of Opus 4.6 I gained (over-)confidence - suddenly every job seemed doable with a good prompt.
- I hit a plateau - some things work, some don't. Hard to say why.

The question why the LLM sometimes works great and sometimes fails miserably kept my brain busy for weeks. Why isn't this working?

## The problems I found

### Comprehension debt

There's a long term cost for moving fast with an LLM - we can't comprehend the work anymore, and the more the project grows the more the LLM's output degrades. We can see this in the small (long conversations) and in the large (weeks/months of work). Turns out this has been mapped already and there's a term for it: [comprehension debt](https://addyosmani.com/blog/comprehension-debt/).

### LLM failure modes

Comprehension debt aside, the SotA LLMs (as of March 2026) have their own issues:

- **they can fill in the gaps in our request, but with the wrong things.** They might choose a bad algorithm, use the wrong function, include a third-party library we didn't request nor approve...
- **they can be confidently wrong.** They will explain in great detail why your code has a concurrency bug, citing the right sources, but actually just fooling you. They will claim there's a function in the standard library solving your problem, but the function doesn't exist, or does something completely different than what you need.
- **they can forget requirements**, or convince themselves the requirement isn't necessary and just downright fail fulfilling the request.

OK, as they say "a problem well-stated is a problem half-solved". We understand we need to build comprehension, we understand the LLM can make mistakes. We framed the problems, now we can look for solutions!

## The pipeline

Let's look at our 2 extremes again, but with a new framing. The vibe coders start with *high velocity* at the expense of understanding the code. The skeptics *keep their understanding* but lose out on the opportunity of using AI to increase their velocity.

So, if we want to increase our velocity we *also* need to keep up with our understanding. How can we do that?

By bringing structure (a pipeline) into our conversation with the LLM. I settled on a simple one:

- frame the problem: what is the problem, what's out of scope
- research: what's already built, how do others do it
- design: unfold a few solutions, converge to one
- spec: describe the chosen solution in detailed, mechanical steps
- code: implement the spec

This pipeline tackles all of the above issues:

- comprehension debt: moving through the pipeline you discover gaps, couplings, possible blockers, non-goals. You learn from research. You pick from alternatives. You read the spec.
- gap-filling: the pipeline uncovers these.
- confidently-wrong LLM: first of all the LLM will need to carry the lie over all of the phases, which is hard. Secondly we'll be reading all of it, and the more we read the easier to get a feel something's off.
- forgetting requirements: you have the spec with very clear definitions. It's much harder for the LLM to disregard them.

When executing this pipeline I hit into various issues. The LLM's built-in prompt primes it to be helpful, by writing code and filling in gaps. It takes the quick route and escapes the phases. I experimented with various prompts to enforce phase boundaries. Here's what I use - [ai-behaviors](https://github.com/xificurC/ai-behaviors). The phases map to hashtags: `#=frame`, `#=research` etc. You drop it in a prompt and the LLM enters the mode with you. It won't escape the mode until you intentionally move to the next one. 

## The proof

I'm using this pipeline to great effect, even on `ai-behaviors` itself. Here's an example of building [composites](https://github.com/xificurC/ai-behaviors/commit/8633aa99d62fdb754503936be029e6cfe74768b6#diff-bb98d815b43eadbdf80e5164a672f1d5261178fd9819a7eb97890e70480bd7f0), a feature to compose N hashtags into a single one. If I were to code this myself my plan was to

- add a parsing step where I'd search for a special line like REQUIRE: #foo #bar #baz
- recursively expand these
- test it manually on some examples
- update the project readme

Going through the pipeline with the LLM I improved the delivered feature:

- I picked a different solution than parsing a line
- I got tests for edge cases
- I got defensive code against fork bombs or accidental cycles

I also got to realize there's a lot of interactions I didn't take into account. Like, how should we persist the active hashtags - use the composite or the expanded set? Where can the user define them - project scope? User scope? How should it work with the #EXPLAIN feature? Should it show the decomposition? Does order of hashtags matter? Should the expansion honor the order?

These are real concerns I did not consider. Without the LLM I would have fumbled into them during implementation, or missed them entirely. This shows I not only increased my velocity, but I also gained a better understanding of the feature.

This is the middle of AI coding - the place where, through structured conversation, increased velocity meets with increased understanding.

## The meta proof

This also works beyond coding - I used the same pipeline when writing this blog post. This post is **100% human written** but I did collaborate on framing, researching, designing and reviewing it with Claude Code. The interaction helped me deliver faster and better, while staying fully in control and learning how to write blog posts at the same time. [Full conversation here](https://gist.github.com/xificurC/d98e1e05232d9f815b897ccaff85f4cc).


## Prior art

I'm not the first to come up with structured conversations, there are [spec driven](https://speckit.org/), [persona based](https://github.com/bmad-code-org/BMAD-METHOD) and [phase managed](https://github.com/gsd-build/get-shit-done/) frameworks already out there. For me they are too heavy-weight, I prefer smaller frameworks.
