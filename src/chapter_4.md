# Mutation and Memory

Mil does not have many higher-level language constructs, so mutation is needed
for even basic things like iterating over a vector. Mutation is also useful for
writing performant code that the compiler itself may not be able to do.

However because of MelVM's persistent data structures, Mil's notion of mutation
is a little nuanced. Basically, variable references

All variables reference a location in memory (the MelVM heap). This location is
determined when a variable is bound in a let-binding. `set!` allows the
location a variable references to be bound to a new value.

```clojure
(let (i 0) ; memory-location, say.. 17.. stores 0 and i refers to it
    (+ i 1)) ; memory location 17 is still 0

(let (i 0) ; memory-location 18 stores 0 and i refers to it
    (set! i (+ i 1)) ; memory location 17 is still 0
```

The general syntax of set!:
```clojure
(set! <variable-name> <expression>)
```

Note that the location a variable references is not mutable. This is stuck for
the lifetime of the binding.

Mil follows a simple incremental scheme for assigning locations to variables.
The first few memory addresses (starting at 0) are reserved for preloaded data
like covenenant hash, etc.. When a variable's scope ends, the memory location
is freed simply by decrementing a memory-head-counter. This is safe because
all variables are scoped; no variables can live past their defined scope.

Also notice that set! does not expect a memory location, but instead a valid
variable. This is an ergonomic design decision in Mil to block arbitrary memory
re-writing. It is possible in the future that Mil will need more
power/flexibility here, and raw memory setting could be enabled to make
pointer variable types possible. However, it is not clear yet that this is
needed since MelVM's persistent data structure interface makes many cases of
mutation unnecessary.
