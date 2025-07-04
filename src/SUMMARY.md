[Tensor Compilers: Zero to Hero](./pref.md)
[Dedication](./dedi.md)
[Overview](./over.md)


# Scalar Compilers: `picoc`
- [AST Compilers: c0/pcc/lcc]()
    - [Standard Implementation Plan]()
        - [IR: Modeling Programs]()
        - [Codegen: Lowering Programs]()
            - [Selector: Greedy Tree Covering]()
            - [Alloctor: Register Pool]()
        - [Parser: Lifting Programs]()
            - [Lexer:]()
            - [Parser: Recursive Descent]()
    - [Ghoulom Approach]()
        - [Calculator: Arithmetic, Bindings](./ch1.md)
        - [Control Flow: Branches, Loops]()
        - [Memory:]()
        - [Hello World: Functions, I/O, FFI]()
        - [Fuzzing: CSmith]()
    - [Next Steps]()
        - [Optimizing Compilers]()
- [CFG(BB) Optimizing Compilers: bril/c1/llvm]()
    - [Optimizer: Manager of Passes]()
        - [CF, CSE, DCE]()
        - [Inline, Unroll, Vectorize]()
        - [Code Motion]()
    - [Codegen: NP-hard heuristics]()
        - [Selector: Greedy Graph Covering]()
        - [Scheduler: __]()
        - [Allocator: Linear Scan]()
- [SoN Optimizing Compilers: c2/libfirm]()
    - [Codegen]()
        - [Optimizer: GVN+GCM]()
        - [Allocator: Graph Coloring]()
    - [Optimizer: Worklist of Rewrites]()
- [Optimizing Optimizing Compilers: rust, carbon, zig]()
    - [Incremental Compilation]()
    - [Parallel Compilation]()
- [Correcting Optimizing Compilers:]()
    - [Semantics]()
---
# Vector Compilers: `picocuda`, `picotriton`
---
<!-- # Tensor Compilers:
- [User Schedulable]()
halide, exo, tinygrad, luminal??? -->
<!-- ---
# Tensor Compilers: picograd
- [Eager Interpreters]()
- [Graph Compilers](./ch5.md) -->
---

# Appendix: Computer Architecture
<!-- [A Scalar Processors](./apa.md) -->

[A. Scalar Processors](./apa.md)
[B. Vector Processors](./apb.md)
[C. Tensor Processors](./apc.md)
[Bibliography](./bib.md)

<!-- # Vector Processors: NEON, SSE, NVPTX
- [Processor Parallelism (Instruction Level) Out of Order Speculative Executions, Superscalar, Branch Prediction]() -->
<!-- - [Macroarchitecture: RISC-V]()
- [Microarchitecture: Single-cycle, Multi-cycle, Pipelined]()
- [Memory:]() -->