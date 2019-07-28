# Type of Paralleism for peak performance modern hardware

Task Paralleism : Multithreading, Multi-core

**SIMD Parrelism: SIMD ISA**

# ISPC
FLOPs without beiging a ninja program

ISPC is short for Intel SPMD Program Compiler

LLVM-based langauge

generate high performance vector code for many vector ISAs
SSE/AVX/AVX2/AVX-512/NEON...(experimental)

# ISPC is not an "autovectorizing" compiler

## It' dosent not generate vector code by analyzing and transformaing scalar loops.

## ISPC is more of a WYSIWYG vectorizing compiler
- The Programmer tells ISPC what's vector and what's scalar
- Vector type are explict, not discovered

# language shape
- It's looks very much like C, so it's easy to read and understand

- code looks sequential but excutes in parallel
    - Easily mixes scalar and vector computation

- Explict vectorziation using two new keywords *uniform* and *varying*
    - Again, ISPC is not an auto vectorzing compiler

(It's basically shared programming for cpu)

# The key concept "uniform" vs "varing"

## Uniform
- scalar data

## Varing
- Vector data
-- Result in SIMD vector register(XMM,YMM,ZMM,etc)
- Varying is the default!
- Each SIMD lane get a unique value


# Why is this good
- Programmer no loger need to know the ISA to write good vector code.
    - More accesible to programmer who aren't familar with SIMD instrincs
    - More programmer able to fully utilize various ares of game develoment
- It's easier to read and maintain
- Supporting new ISA is as easy as changing command line option and recompiling
- It's most common to achieve 3x speedup on 4-wide SSE units and 5-6x on CPU's with 8-wide AVX2 units without the difficuly of writing Intrinsics.

# where can i use it?
- Physcis (Chaos demo)
- Particle System
- Skning
- Asset pipeline(huge benefit)
- compression/decompression
- raycasting
- intersection testing
- image convolution
- post processing
- software culling

# Demo
- particle
- SPH

# ISPC Language

https://github.com/ispc/ispc