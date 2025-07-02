[Tensor Compilers: Zero to Hero](./pref.md)
[Dedication](./dedi.md)
[Overview](./over.md)

<!-- https://www.youtube.com/watch?v=Qv-wXcUxrmE
https://www.spinellis.gr/blog/20060626/
https://rustc-dev-guide.rust-lang.org/overview.html?highlight=refcell#parallelism
https://rustc-dev-guide.rust-lang.org/query.html
https://clang.llvm.org
https://www.youtube.com/watch?v=ZI198eFghJk&t=6s
https://www.sigbus.info/how-i-wrote-a-self-hosting-c-compiler-in-40-days -->

# Scalar Compilers:
- [Compilers: AST (tcc/chibicc/acwj)]()
    - [Translator: Parser, Generator]()
    - [Calculator: Arithmetic, Bindings](./ch1.md)
    - [Control flow: Branches, Loops]()
    - [Memory:]()
    - [Hello world: Functions, I/O, FFI]()
    - [CSmith]()
- [Optimizing Compilers: CFG(BB) + SSA (c1/llvm)]()
    - [Frontend: Parser]()
    - [Middleend: Optimizer]()
    - [Backend: Generator]()
- [Optimizing Compilers Redux: SoN (c2)]()
- [Optimizing Optimizing Compilers: Incremental, Parallel, Arenas]()
<!-- SoN does the same thing as SSA (shows dataflow) but with effects(LOAD/STORE) and control-->
<!-- SoN generalizes SSA. generalizes (more graph theory) graph is way less contrained. more legal orderings ==> payoff: you can organize(rewrite) the graph anywayyou want-->
<!-- as long as dependencies remain the same. -->
<!-- SoN: is the enlightenment of IR -->
<!-- SoN: not good for specualitve optimizations (todo: mppd). requires scheduling pass (GCM?) that's expensive. -->
<!-- schedule can mess up your CFG. so you need a very smart scheduler. -->
<!-- problem with team dynamics. people do an optimization (edge is missing), and scheduler fucks it up. you can get miscompile or perf hit-->
<!-- specualtive optimization: using dynamic information (js) -->
<!-- SoN can be slower because more edges. bigger in memory. SON compilers usually slower than CFG compilers? -->
---
# Vector Compilers: picocuda
---
# Tiling Compilers: picotile
<!-- ---
# Tensor Compilers: picograd
- [Eager Interpreters]()
- [Graph Compilers](./ch5.md) -->
---

# Appendix: Sources and Targets

<!-- [Mathematics](./apa.md) -->
[A Scalar Processors: RISCV]()
[B Vector Processors: NEON, SEE, NVPTX]()
<!-- # Vector Processors: NEON, SSE, NVPTX
- [Processor Parallelism (Instruction Level) Out of Order Speculative Executions, Superscalar, Branch Prediction]() -->
<!-- - [Macroarchitecture: RISC-V]()
- [Microarchitecture: Single-cycle, Multi-cycle, Pipelined]()
- [Memory:]() -->
[C Tensor Processors: ]()
[D Tensor Programs: llama2, r1](./apd.md)

[Bibliography](./bib.md)