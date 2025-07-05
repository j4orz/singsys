# Course Overview

## Machines
The brain is an existence proof of physics powering `20PFLOP` machines
with `20W` in the natural world. However, the *semiconductor* physics of *today*
is hitting against walls which power the machines of the artificial world.
The walls are two-fold:
1. instruction-level parallelism from out-of-order superscalar pipelines is hitting diminishing returns
2. frequency scaling is hitting against [Dennard scaling](https://en.wikipedia.org/wiki/Dennard_scaling)'s power wall

and so the free lunch with respect to single threaded performance
([Moore's law](https://en.wikipedia.org/wiki/Moore%27s_law)) that transitioned
us across computer classes — from minis to micros to mobile — is "over" (for now).

As a result, computer architects moved from scalar to vector processors by
building multi/many-core machines. These days, to unlock the full performance of
hardware, it's the programmer's responsibility to parallelize their software.
Expressing non-trivial problems so that they map well parallel machines is complex
enough to make it worth a research paper. The problem with the vector processing
of multi-core/many-core machines is two-fold:
1. programming model: compilers were [sufficiently smart](https://wiki.c2.com/?SufficientlySmartCompiler) with [autovectorization](https://pharr.org/matt/blog/2018/04/18/ispc-origins)
2. execution model: program speedups were bound by [Amdahl's law](https://en.wikipedia.org/wiki/Amdahl%27s_law)

which were sidestepped, respectively, by:
1. modifying the programming model from SIMD to SIMT (TODO: macroarch vs microarch)
2. finding domains that were compute-bound that desperately required the parallelism offered by GPUs

This aforementioned domain turned out to be that of deep neural networks — the
statistical approach to artificial intelligence. These massively parallel,
differentiable programs enable the stochastic, continuous, software 2.0
models, which are the distant cousins to the deterministic, concrete, software
1.0 algorithms.

And so throughout the past decade, the infrastructure responsible for powering AI
has rapidly evolved to meet the needs of deep neural networks — most notably with
the throughput performance of GPUs moving from `TFLOP/S` (1e12) to `PFLOP/S` (1e15).
As we speak — in the last year of the first quarter of the 21st century — the
supercomputing industry is capable of building machines (that are non-distributed)
that reach speeds of `EFLOP/S` (1e18).

## A New Golden Age
agricultural -> energy -> information -> intelligence
- in the same way it would be a diservice to teach computer architecture course
with a single pipeline, connection to the frontier. to research.
this is a reimagined compiler course for the age of heterogenous compilation.

The challenge (producing a golden age) of compiler engineers and chip architects face is to find the optimal mapping from
intelligence to energy. This means creating new programming languages and machiens
while minimizing the accidental complexity that naturally builds up along the way:

- [The Golden Age of Compiler Design (Lattner)](https://www.youtube.com/watch?v=4HgShra-KnY)
- [A New Golden Age for Computer Architecture (Hennessy and Patterson)](https://www.youtube.com/watch?v=3LVeEjsn8Ts)

## Course Outline

pointwise compilers.
C->RISCV
CUDAC->PTX

Part 1: `picoc` compiles `llm.c`

**Scalar Compilers**
- AST Compilers with c0 (pcc/lcc)
- CFG(BB) Optimizing Compilers with bril (c1/llvm)
- SoN Optimizing Compilers with Simple (c2/libfirm)

**Vector Compilers**
...

pointwise compiler:
- numpy/torch -> PTX
Part 2: `picograd` evalautes and compiles (TODO: jit compiles?) `nanogpt.py`