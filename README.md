# VALUE-SEMANTICS-UTILS

Utilities for using CLOS for mostly-functional programming.

## Why?

There are two worlds in programming: an imperative one, full of objects and comparisons by identity, and a functional one, full of referential transparency and comparisons by value. Common Lisp is good at both, and I ended up realizing that I wanted to develop a program which integrates both of these paradigms.

In particular, I want my data structures to be mostly immutable and the logic of my program to be pure and without side effects, but I'd like to structure my data via standard classes and have the possibility to mutate objects wherever it cannot be observed in the higher-level logic.

For such a programming style, the following assumptions should hold true:

* Data should be immutable and structure sharing should be common wherever possible.
  * It should be possible to use imperative logic wherever doing so does not mutate existing data.
* Data should be represented via primitive Common Lisp types and standard classes.
  * It should be possible to use cyclic references for programming convenience.
* It should be possible to consider a type mismatch an abnormal situation while comparing for equivalence.
  * It should be possible to use `:type` arguments for class slots and have runtime assertions for type checks without risking undefined behavior or depending on implementation-defined behavior.
* Value semantics should be used to compare data for ~~equality~~equivalence. In particular, two classes should be equivalent if their types and contents are equivalent.
  * The only exception to using pure value semantics should be cycle detection in data structures, for which there seems to be no solution better than identity comparison.
    * (Thankfully, there are no generators or infinite lists in Lisp.)
* There should be a way to ensure, on the MOP level, that a slot in an instance is...
  * ...meant to be always bound,
  * ...meant to always contain a value of a particular type.

This repository contains a series of utilities meant to facilitate this style of programming.

## Manual

There are three sources of authority for the code in this repository:

* the [`EQV` manual](doc/EQV.md),
* the [classes manual](doc/CLASSES.md),
* the [test suite](t/) containing more examples and edge cases.

## License

MIT.

## Tested on

SBCL 2.1.11 with some custom fixups. Nowhere else, yet. Expect breakage because of high doses of MOP wizardry, even though this library uses `closer-mop`.

On SBCL, we need to wait for https://bugs.launchpad.net/sbcl/+bug/1956621 to get fixed ~~and for the [patch](https://sourceforge.net/p/sbcl/mailman/sbcl-devel/thread/6ae094ba-eeea-6bfe-b43d-970d97040830%40disroot.org/) that stabilizes behavior for failed `U-I-F-{R,D}-C` to get merged~~. Sigh. MOP is hard. MOP interactions with everything else are even harder.

## Lemme in, LEMME IIIINNNNNNN

If you don't want to wait for SBCL to catch up, evaluate [this](sbcl-fixup.lisp) in order to get the tests to pass. Trust me, I'm an engineer.
