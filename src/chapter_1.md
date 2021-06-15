# Introduction

Mil, short for the MelVM Intermediate Lisp, is a small language designed to
reflect the MelVM internal architecture, while also providing a few common abstractions
for complex programs. If you are reading this, you are either:

1. Designing a higher level language for MelVM and you want to compile to Mil as ankk
intermediate language. Or you need some expressive power (probably for optimizations)
that may not be available in a higher level language.
2. Developing/improving aspects of Mil itself.

Each chapter will start as a user guide to a certain feature of mil. Then
implementation details will be provided in section labeled "Language Comments".
As the mil language is developed, desgin changes/decisions and new features will all
be documented here.

Keep in mind that Mil is generally not a friendly language to be used directly by
programmers, but instead to be an intermediate language which higher level
languages can compile to. Other languages will benefit from existing compiler
optimizations and basic abstractions by compiling to Mil instead of directly to binary.

Don't be too put off or excited that Mil is a Lisp. It is only in the sense
that it is a language of S-expressions. There is no meta-evaluator or any fancy
macros.

The MelVM is designed with functional data structures in mind, using RRB trees
in memory (you can read more about the [MelVM
here](https://docs.themelio.org/specifications/melvm-specification/). So
Mil builtin functions feel very functional, in that they return new data
instead of mutating the data passed in. This is a direct reflection of MelVM
instructions.

However, Mil does support mutability with respect to the address that a
variable references. That is, a variable `x` that references address 1, can be
`set!` to address 2 without being re-bound. This is made safer by not allowing
a direct set to an address, but instead set only allows a variable to reference another variable or expression.
