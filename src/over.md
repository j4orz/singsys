<!-- ![](./au197.jpg) -->

# Singularity Systems Overview

**Contents**
- [Singularity Overview]()
- [Systems Overview]()

## Singularity Overview — Software 2.0

## Systems Overview — "Golden Age" Infrastructure Buildout


implementing 4 compilers might be intimidating.
In the same way artists paint over and over,
and mathematicians rederive over and over, language implementors should
represent over and over.

```
                  ,--.    ,--.
                 ((O ))--((O ))
               ,'_`--'____`--'_`.
      _|---------------------------------|_             to the tensor
     | |Tensor Compiler: Torch  -> Triton| |                  ^
     | |Tiling Compiler: Triton -> PTX   | |                  ^
     | |Vector Compiler: CUDA   -> PTX   | |                  ^
     | |Scalar Compiler: C      -> RISC-V| |                  ^
     | |---------------------------------| |           passing assembly
     | |:::::::::::::::::::::::::::::::::| |                  ^
     | |::::::::::::::µarch::::::::::::::| |                  ^
     |_|:::::::::::::::::::::::::::::::::|_|                  ^
       |---------------------------------|           from the transistor
            __..-'            `-..__
         .-| : .----------------. : |-.
       ,\ || | |\______________/| | || /.
      /`.\:| | ||  __  __  __  || | |;/,'\
     :`-._\;.| || '--''--''--' || |,:/_.-':
     |    :  | || .----------. || |  :    |
     |    |  | || '----SSt---' || |  |    |
     |    |  | ||   _   _   _  || |  |    |
     :,--.;  | ||  (_) (_) (_) || |  :,--.;
     (`-'|)  | ||______________|| |  (|`-')
      `--'   | |/______________\| |   `--'
             |____________________|
              `.________________,'
               (_______)(_______)
               (_______)(_______)
               (_______)(_______)
               (_______)(_______)
              |        ||        |
              '--------''--------'
```

**Course Information**
Singularity Systems: Zero to Hero follows up from
[Neural Networks: Zero to Hero](https://karpathy.ai/zero-to-hero.html). We convert
- [**micrograd**](https://github.com/karpathy/micrograd): toy backpropagation engine *into...*
- [**picograd**](https://github.com/j4orz/picograd): modern deep learning framework

While micrograd helps research scientists to understand the leaky abstraction of
backpropagation, picograd is intended for systems programmers and performance
engineers looking to further understand the compilers and chips of deep learning.

Try it out with:
```
pip install picograd
```

**Prerequisites**
- solid deep learning (llama)
- solid systems programming (C || C++ || Rust)

**Syllabus**
**Core**: *Deep Learning Compilers*

1. **dfdx(nd)**: implements an interpreter for neural networks (HIPS/autograd)
2. **brrr**: accelerates the interpreter with vector processing (pytorch1)
3. **pt2**: constructs a compiler for neural networks (pytorch2)
4. **az1use1**: 3d parallelism

Throughout the past decade, modern day AI infrastructure has rapidly evolved
to meet the needs of deep neural networks — most notably with the throughput
performance of GPUs moving from `TFLOPS` to `PFLOPS`. Datacenter
computing now has the goal of machines with `EFLOP` speeds, now that that the
throughput of the fastest (non-distributed) supercomputers on TOP500 LINPACK
workloads are just reaching `EFLOP` levels.

Although the brain is an existence proof of physics powering `20PFLOP` machines
with `20W`, the problem with the semiconductor physics of *today* is two-fold:
1. instruction-level parallelism from out-of-order superscalar pipelines hits diminishing returns
2. frequency scaling is hitting against [Dennard scaling](https://en.wikipedia.org/wiki/Dennard_scaling)'s power wall

and so this free-single-thread-performance-lunch aspect to [Moore's law](https://en.wikipedia.org/wiki/Moore%27s_law)
that transitioned us across computer classes from minis to micros and from micros
to mobile is "over".

As a result computer architects are moving from homogenous general hardware
to heterogenous specialized hardware, which means that the complexity of extracting
program performance leaks upwards from the hardware — these days, to unlock
the full performance of hardware, it's the programmer's responsibility to program
the vector processors in multi-core/many-core machines.

The problem with the vector processing of multi-core/many-core machines is two-fold:
1. programming model: compilers were [sufficiently smart](https://wiki.c2.com/?SufficientlySmartCompiler) with [autovectorization](https://pharr.org/matt/blog/2018/04/18/ispc-origins)
2. execution model: program speedups were bound by [Amdahl's law](https://en.wikipedia.org/wiki/Amdahl%27s_law)

But the industry sidestepped these problems by changing the programming model to
SIMT on SIMD (CUDA) and finding domains whose execution models had more parallelism
(deep neural networks).

The challenge (producing a golden age) of compiler engineers and chip architects face is to find the optimal mapping from
intelligence to energy. This means creating new programming languages and machiens
while minimizing the accidental complexity that naturally builds up along the way:

- [The Golden Age of Compiler Design (Lattner)](https://www.youtube.com/watch?v=4HgShra-KnY)
- [A New Golden Age for Computer Architecture (Hennessy and Patterson)](https://www.youtube.com/watch?v=3LVeEjsn8Ts)

**Singularity Systems**

The [Singularity Systems: Zero to Hero](./syllabus.md) course follows up from
where [Neural Networks: Zero to Hero](https://karpathy.ai/zero-to-hero.html)
left off: we will convert `micrograd` into `picrograd`, where the main difference is that:
- [micrograd](https://github.com/karpathy/micrograd) is a backprop engine with scalar-support helping researchers  understand that backpropagation as an abstraction is in fact leaky (gradient activations, normalizations)
- [picograd](https://github.com/j4orz/picograd) leans closer towards modern day deep learning frameworks with tensor-support for both pytorch1 interpretation and pytorch2 compilation.

While `picograd` is more oriented towards low-level system programmers and
performance engineers, this framework remains *pedagogical* and remains a
point-wise compiler. This means that we will "only" support:
- **1 model**: llama
- **1 programming model**: eager
- **2 execution model**: eager, graph
- **2 hardware architectures**: amd cpu, nvidia gpu
- **2 precisions**: fp32, tf32

*Welcome to the golden age of Systems ML!*

<!-- ### 1(0) year version
Although the Singularity Systems course points towards tinygrad as a system
attempting to ammend the industry's lack of *performance-portability* (removing the
"cuda moat" by "comoditizing the petaflop"), there are other possible solutions
that take a broader view. Rather than building a new framework or domain specific
language, perhaps new programming and execution models implemented as new
languages and machines for parallel computing should be built.

And this is exactly what companies like Mojo and Tenstorrent are doing — they are
re-industrializing research ideas from the unsolved world of parallel computing.
The former riffing on ideas from comptime metaprogamming pioneered by Zig, and
the latter on ones from CELL/Larabee tensor processing. -->


<!-- Jax+XLA is better on TPUs.
PyTorch+Inductor is better on NV.
both have poor performance (for complex workloads) AMD.

fragmentation. no shared
accidental complexity in non-differentiated components ==> FLOPS/W or FLOPS/$
hardware is getting harder.
tt chasing sanity. amd following nv. todo: jim keller economic law?? ai first time it didn't get cheaper?
nv: "moat". "Swamp" of kernel authoring. sidestepped the issue. potential to bite them in butt. impedence mismatch.
todo: mojo 9part blog
the accidental complexity of impedence mismatch (programming graphs on vector processors)
is making it non-general

general: is when a library is modular so you can mix-match the components for applications
that the library designer never thought of.
for instance: LLVM as a modular compiler infrastructure. Clang (OpenCL), Swift Julia Rust etc.
what if there are other graph-related workloads that 

what this course is going to introduce today's solutions. subset deep learning frameworks.
but will end up exploring preparing you for the future of tomorrow. mojo and tenstorrent?
unsolved problem of parallel computing.
-> raph levien's notes and links.

course is only covered to a single machine (scale up)
perhaps a v2 course offering can provide multi-machine distributed systems (scale out).
-> intersection of graph compilers and distributed systems.

programming an M1 and a whatever1 feel relative the same.
the hardware/software contract is pretty clean for scalar computing
and does not leak complexity up the stack.

the same cannot be said for unlocking the performance of modern day for
vector processors and tensorprocessors. you have to know about (come back here:
TMA, X, Y, Z)





So although the course "only" builds a point-wise solution, we will make the
connections to how production-level language implementations (PyTorch and friends)
handle the problem of generitization so tha

Deep learning (and graphics) are enjoying 10-100x more perf(throughput) of GPUs/TPUs,
but it's very expensive so can we make it more cheaper (efficient) `FLOP/W` or `FLOP/$` for existing AI/graphics workloads?
-> which might have downstream effects of unlocking FLOP/S to any compute that
can be expressed as a graph.

programming model implemented by interpreter/compiler. "only" achieving 50% utilization
execution model implemented by chip: graph programs on vector processors

CONCLUSION: lots of accidental complexity arisen (genius workaround at first but now ducktaping)

SOLUTIONs today: graph compilers, kernel authoring: CUBLAS, CUDNN,
SOLUTIONS tmr: mojo/apl/futhark (metaprogramming with comptime), tenstorrent/cell (dataflow computing)

we will build a poitn-wise (1 model, 1 framework, 1 hardware) solution picograd
built that were hillclimbed from yesterdays solutions:

*Welcome to the golden age of Systems ML!* -->