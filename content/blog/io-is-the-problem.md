+++
title = "IO is the Problem"
date = 2026-05-06
+++

Functional core, imperative shell. Haskell's IO monad. Clojure's immutable data structures. What's common in all of these?

They sidestep the problem, which is IO. Reading from disk. Talking to the database. Network hops. The "solutions" above do one thing - put a brick wall around a part of your code and say "look how much we simplified programming!".

When you use them, how do they actually help with IO? I know how to write a pure data pipeline. Everyone does after a year of programming. What we need is help with IO, which is where the complexity lies!

Knowing the question is half the problem. Know this - IO is the problem. Functional programming won't save you. Monads won't save you. What will?
