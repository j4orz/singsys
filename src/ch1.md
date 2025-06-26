# 1. A Batch of Dragons: Parser, Optimizer, Generator

**Contents**

*And Dennis said, "Let there be C" and [there was C](https://en.wikipedia.org/wiki/The_C_Programming_Language).*

Some of the first C compiler designs which involved preprocessing, lexing,
parsing, code generation, assembling and loading were described in
[(Johnson 1979)](https://c9x.me/compile/bib/pcc-tour.pdf).
Shortly after, this three phase design with parsing at the front, optimizing in the
middle, and generating at the back was compiled into the seminal
["dragon book"](https://en.wikipedia.org/wiki/Compilers:_Principles).

![](./dragonbook.jpeg)

The first chapter of the textbook implements a compiler for a subset of K&R C
with the dragon book's three phase batched design with the incremental approach
presented in [(Ghuloum 2006)](http://scheme2006.cs.uchicago.edu/11-ghuloum.pdf).
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
for instance is that of a compass, but the interesting machines are those
that are capable of evaluating more expressive languages formalized as
[Turing complete](https://en.wikipedia.org/wiki/Turing_completeness). While
in theory processors are able to execute anything such as the
Lisp machines [(Moon 1985)](https://dl.acm.org/doi/pdf/10.5555/327010.327133),
what happens in practice is that the languages used to program computers are
closer to the metal. That is, they reflect the underlying hardware components,
like registers for instance. These languages are referred to as assembly, their
words as instructions, and their vocabulary as instruction set architeture (ISA).

All modern day ISAs owe spiritual debt to the family of System/360 mainframes,
made by International Business Machines. They were the first computers to unify
under a single ISA [(Amdahl, Blaauw, Brooks, Jr 1964)](https://www.ece.ucdavis.edu/~vojin/CLASSES/EEC272/S2005/Papers/IBM360-Amdahl_april64.pdf) where the ISA reference card
became infamously known as the ["green card"](http://archive.computerhistory.org/resources/access/text/2010/05/102678081-05-01-acc.pdf).

![](./ibm360.jpg)

While there are many points and tradeoffs in the space of ISA design, the textbook
defaults to what is most open: RISC-V [(Asanović, Patterson 2014)](https://people.eecs.berkeley.edu/~krste/papers/EECS-2014-146.pdf), [(Waterman 2016)](https://people.eecs.berkeley.edu/~krste/papers/EECS-2016-1.pdf), and [(Patterson, Waterman 2017)](http://riscvbook.com).

Returning to the original goal of the computer
— bridging the semantic gap between humans and electrons — assembly code is indeed
more ergonomic than expressing programs in machine code (raw binary encodings),
but there is a higher level of semantics which is even more ergonomic than that of
assembly. Modern day programming languages such as Java, Python, Lisp.
start with C, the portable assembly. The purpose of the compiler is to translate C to RISC-V.


```
     o
 _ /<.
 (*)>(*)
--------- C
 compiler
--------- RISCV
processor
---------
 e- e- e-
```

## 1.2 Here be dragons
Abstract syntax















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