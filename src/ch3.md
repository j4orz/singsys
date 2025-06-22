# B. Computing, Chips, Compilers

**Contents**
- [B.1 Chips: Physical Evaluators]()
    - [Macroarchitecture: RISC-V]()
    - [Microarchitecture: Single-cycle, Multi-cycle, Pipelined]()
    - [Processor Parallelism (Instruction Level) Out of Order Speculative Executions, Superscalar, Branch Prediction]()
    - [Memory:]()
- [B.2 Compilers: Virtual Translators]()
    - [Intermediate Representations]()
    - [CFG+BBs: LLVM (Clang, Swift, Julia, Rust)]()
    - [SSA?]()
    - [SoN: JVM's Hotspot (C2), v8's Turbofan, PHP 8, libfirm]()
        - [Incremental Compilation: Frontend (Parser), Middleend (Optimizer), Backend (Generator)]()
        - [Arithmetic]()
        - [Aliasing]()
        - [Control flow]()
        - [Heap]()
    - [egraphs: cranelift]()
        - [Incremental Compilation: Frontend (Parser), Middleend (Optimizer), Backend (Generator)]()
        <!-- https://github.com/mwillsey/cs265/tree/2024-fall?tab=readme-ov-file
        https://inst.eecs.berkeley.edu/~cs294-260/sp24/ -->
- [B.3 Performance Analysis and Tuning: Bottlenecks]()
    - [Memory Bound: Cache Misses]()
    - [Compute Bound: Parallel....]()
    - [Bad Speculation: Branch Mispredictions]()
    - [Frontend Bound]()

note to self:
- zip the stack from the bottom up: code generator for riscv. do not implement the uarch for riscv core.
- compiler first. then performance analysis.
- even if it's not necessarily the case that all performance engineers have compiler experience,,
it's nice to see the common optimizations a modern day optimizting compiler makes.

NB. SoN is the third:
    a. tree: precedence is represented via tree's hierarchy.
    b. two-tiered nested graph of basic blocks of instructions: edges denote ONLY control flow
    c. single-tiered flat graph of instructions: edges denote control flow OR data flow
       SoN makes dependencies explicit and schedule(order) implicit
       decouple control from data. SoN philosophy says why do we have so many
       data embdedded in control chain?

# B.1 Chips: Physical Evaluators

### Macroarchitecture: RISC-V

processor and memory


### Microarchitecture: Single-cycle, Multi-cycle, Pipelined

- general purpose registers
- load/store architecture

architecture is the programming model "view" of the computer.

von neumann architecture "5" components:
(preliminary discusion of logical design of electronic computing instrument 1946)
- processor: control + data
- memory (instructions + data)
- i/o

.. PROCESSOR: evaluates the instruction (computes)
.. --------------------------------------------------------------------------------



.. MEMORY
.. --------------------------------------------------------------------------------
most von neumann architectures are byte adressable.
changing with machine learning. fp4. fp2?? energy requirements tradeoff.

word addressable (32/64bits at a time) 4hex/8hex digits
byte addressable (8bits=1 byte at a time)

how are the bytes in a word ordered? little endian (MSB <- LSB)/big endian (LSB -> MSB).



.. key(address) value(word)
.. ------------------------
000            u8
001            u8
010             .
011             .
100             .
101
110            u8

#### Single-cycle

state: sequential logic
processor: combinational logic (ALU)
- datapath
- control unit

core (processor):
4 stateful elements
core: 1. processor counter 2.registers (FETCH/load.DECODE)
memory: 1. instruction memory 2. data memory (STORE)
- (instr. mem is ROM for now)
- the multicycle uarch will have more realistic unified memory (instr + data)


design process
*start with stateful elements (LOAD/STORE)
add blocks of combinational (functional) logic (EXEC)
to evaluate state_i -> p(instr_i, data_i) -> state_i+1


---------------------------------------
instruction processing loop.
takes single machine clock cycle.
in single-cycle machine.
    - FETCH
    - DECODE
    - EXEC
    - STORE

 s1 --P(i0)-> s0
 |             |
 ---------------
single-cycle machine:  1 instr/clock
(no intermediate programmer invisible states)
cons: slowest instr. determines cycle time.

contra multi-cycle machine: 1 instr/N clocks
- updates made *during* instruction execution
- slowest "stage". determines cycle time.
-----------------------------------------





#### Multi-cycle

#### Pipelined

### Memory

# B.2 Compiler: Virtual Translators
## Specialized JITs (Sea of Nodes): Hotspot's C2, v8's Turbofan, php's opcache, libfirm
v8's turbofan: https://v8.dev/docs/turbofan
jvm's hotspot: https://github.com/openjdk/jdk/tree/master/src/hotspot
php's opcache: https://github.com/php/php-src/blob/master/ext/opcache/jit/README.md
libfirm: https://libfirm.github.io


### Frontend: Parsing
### Middleend: Optimization
### Backend: Generation

## egraphs equivalence term rewriting: cranelift
### Frontend: Parsing
### Middleend: Optimization
### Backend: Generation


# B.3 Performance Analysis and Tuning
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
https://nnethercote.github.io/perf-book/benchmarking.html