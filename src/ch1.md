# 1. A Batch of Dragons: Parser, Optimizer, Generator

**Contents**

*And Dennis said, "Let there be C" and [there was C](https://en.wikipedia.org/wiki/The_C_Programming_Language).*

Some of the first C compiler designs which involved preprocessing, lexing,
parsing, code generation, assembling and loading were described in
[Johnson 1979](https://c9x.me/compile/bib/pcc-tour.pdf).
Shortly after, this three phase design with parsing at the front, optimizing in the
middle, and generating at the back was compiled into the seminal
["dragon book"](https://en.wikipedia.org/wiki/Compilers:_Principles).

![](./dragonbook.jpeg)

The first chapter of the textbook implements a compiler for a subset of K&R C
with the dragon book's three phase batched design with the incremental approach
presented in [[Ghuloum 2006]](http://scheme2006.cs.uchicago.edu/11-ghuloum.pdf).
The incremental approach of implementing the compiler feature by feature
dumbs the dragon down into silly little dwarves — each piece easily understandable.
The first chapter sets up the skeleton for the entire compiler, and the subsequent
chapters implement a programming language with progressive complexity: capable
of performing calculation, control flow, and finally memory access. After the
first dragon is slain, another head grows, and the second
chapter of the textbook covers incremental/query-based compiler design in
compilers like `rustc`. That is, incremental in the compiler's phase order, not
the writer's implementation order.

## 1.1 Soul of the machine
From a systems perspective, a computer is a von Neumann machine
(see [Appendix A](./apa.md)) that bridges the semantic gap between humans and
electrons — a computer orchestrates electrons in order to evaluate algorithms
(a list of instructions). A machine whose computation is more limited in scope
for instance is that of a compass, but the more interesting machines are those
that are capable of evaluating more expressive languages formalized as
[Turing complete](https://en.wikipedia.org/wiki/Turing_completeness). The IBM/360
was the one of the first families of computers to support the same
instruction set architecture (ISA) and infamous ["green card"](http://archive.computerhistory.org/resources/access/text/2010/05/102678081-05-01-acc.pdf)

![](./ibm360.jpg)


```
     o
 _ /<.
 (*)>(*)
--------- IBM/360
processor
---------
 e- e- e-
```















<!-- NB. SoN is the third:
    a. tree: precedence is represented via tree's hierarchy.
    b. two-tiered nested graph of basic blocks of instructions: edges denote ONLY control flow
    c. single-tiered flat graph of instructions: edges denote control flow OR data flow
       SoN makes dependencies explicit and schedule(order) implicit
       decouple control from data. SoN philosophy says why do we have so many
       data embdedded in control chain? -->


<!-- # B.3 Performance Analysis and Tuning
- lab(micro)benchmarks: eliminate nondeterminism and noise
- prod benchmaks: include nondeter. and noise. execution/wall clock time (latency)
noise variance with clock freq (DFS: dynamic freq scalignpower scaling)
    ==> variance with execution time (wall clock time)
    - tools like temci??? exist to reduce variance
- hotspots

"active benchmarking": *guilty* until proven innocent

https://rcs.uwaterloo.ca/~ali/cs854-f23/papers/onelevel.pdf
criterion.rs
https://rcs.uwaterloo.ca/~ali/cs854-f23/papers/topdown.pdf
https://github.com/KDAB/hotspot
https://nnethercote.github.io/perf-book/benchmarking.html -->