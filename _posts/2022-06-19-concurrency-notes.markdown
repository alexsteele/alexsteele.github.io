---
layout: post
title:  "Concurrency Notes"
permalink: /:title/
date:   2022-02-22 00:00:00 -0800
---

*Notes on key concurrency concepts, from the barebones basics up*

## Background

### Computation
*the mathematical model*

* A **universe** is a set of objects. (e.g. the natural numbers)
* A **function** is a transformation from input objects to output objects in a universe. (e.g. 3 + 3 = 5)
* A **computation** is the execution a function according to a well-defined model.
  * Example: The process of dividing 3983 by 8 using long division
* A **model of computation** describes how a function is computed given an input.
  * Example: a **Turing machine** consists of an instruction tape, memory tape, and head pointing to the current instruction.
  * Example: the **Lambda Calculus** consists of terms including variables, abstraction (function definition), and application (applying abstractions to variables)
* A function is **computable** if it can be computed by a turing machine given infinite time and memory
  * Not all functions are computable!
* A model of computation is **universal** if it can compute any function that a Turing machine can
  * **Church-Turing Thesis** - every physically computable function can be computed by a Turing machine or the lambda calculus  
 
### Time
*Time is what prevents everything from happening at once. - Mark Twain*

* Functions are about *what*. Computation is about *how*.
* On a Turing machine, a computation consists of a (possibly infinite) series of machine states from the starting state to the final state.
* On an modern CPU, computation consists of a sequence of electrical states in transistors on a silicon chip.
* What does this imply?
  * Computation takes _time_ - the intermediate states between input and output.
* A computational **process** is an instance of a computation occurring (the states of the computation over time)
  *  Example: the execution of 13935 / 354 on your desktop calculator
* In our universe, multiple computations can happen at the same time. The time of two computations can overlap. Parallelism!

## Key Ideas

* **Parallelism** - doing multiple things at the same time
* **Concurrency** - doing multiple things in the same time *period*

Parallelism != concurrency

* Parallelism implies concurrency
* Concurrency does not always imply parallelism
* Why? You can execute tasks concurrently by switching between them 

Example - unloading a dryer and folding clothes
* Parallelism - One person unloads the drier and another folds clothes 
* Concurrency - One person unloads one clothing item at a time, folds it, then repeats

## Why care?

**The death of Moore's law - multiple cores** - Processors are not getting much faster
  anymore. For high performance, modern processors have many compute
  cores. This allows us to do more things at the same time, but we
  cannot do a single task any faster than before. So to perform tasks
  more quickly, we must design them to use multiple cores.

**Scaling up - distributed systems** - We use multiple computers to store and process more data than a single computer can handle. This implies concurrency as multiple computers do things at the same time.

**IO** - IO is slow. Sending and receiving data from a disk or network typically takes orders of magnitude more time than executing instructions on a CPU. To keep processors busy, they need to do other things while IO is happening.

## Control and Data

* **control** - deciding what to do and when
* **data** - sharing information between processes

## Data

* **atomicity** - updating a data item in a single operation that cannot be interleaved with operations from other processes
* **data races** - multiple processes accessing data in ways that lead to unwanted behavior
* **critical section** - part of a program that needs to be protected to avoid concurrent access to avoid data races
* **mutual exclusion** - guarantee that only one process can access a resource 
* **deadlock** - multiple processes depend on eachother to continue, preventing progress

## Computational Primitives

* **OS Processes** - Processes are isolated and do not share memory by default. Multiple processes can run on a computer
* **OS Threads** - Threads execute within a process and share a memory address space. Multiple threads can run within a single process. On linux, threads and processes are tracked with the same task data structure.
* **Green threads** - Threads which are multiplexed onto operating system threads by a programming language runtime. Multiple green threads can run on the same OS thread.
* **Fibers** - Lightweight threads that use cooperative multitasking. Multiple fibers can run on a thread.
* **Coroutines** - Routines that can yield control back and forth to eachother.

## Coordination

### Approaches

* **Shared memory** - Processes share memory and communicate data and control information by reading/writing memory
* **Message passing** - Processes send messaging to eachother
  * Often easier to avoid data races.
  * **Communicating Sequential Processes** - Tony Hoare's model. implemented by Go's goroutines with channels.


### Primitives

* **CAS (compare-and-swap)** - Atomically compares the contents of a memory cell with a value and replaces it with another value if the two match. Used to build other control primitives.
* **Atomics** - Atomic data primitives that can be read and written in a single instruction with no interleaving between processes.
* **Semaphore** - Counter used for synchronization. Operations:
  * wait: Decrement counter and wait until it's >= 0
  * signal: Increment counter and wake waiter if counter is 0.
* **Condition variable**
* **Lock** - Prevens concurrent access to shard resources. Only one process can hold lock at a time.
* **Mutex**
* **Spinlock** - Acquiring busy-waits CPU. Typically used only in OS.
* **Barrier**
* **Memory barrier** - Guarantees certain memory consistency for a sequence of operations.

## Hardware

### Modern Hardware

* **CPU** - silicon wafer that executes instructions
  * Often multiple CPUs per computer
  * Often multiple **cores** per-cpu can execute tasks in parallel
* **Memory** - Data store accessed by address. Multiple levels. Often: Per-core L1 cache, per-cpu L2 cache, shared L3 cache, main memory.
  * **TLB cache** - Maps virtual memory addresses to physical addreses
* **IO Bus** - Bus to send instructions and data to peripherals.
* **Storage** - Hard drive disk or solid state drive. Data typically transferred in blocks. Common block size of 512 bytes.
* **Network** - Network card to send and receive data from other machines

### Challenges

* **Memory consistency** 
  * Data caches get out of sync.
  * Operations can be re-ordered by hardware.
  * Operations may not be atomic.
* **NUMA** - Non-uniform memory access. Memory access takes different time for different cores.
* **Cache misses** - Multiple causes. Cold misses on first access. Working set > cache size.
* **Context switches** - Latency = ~5000X instruction latency. Tasks compete for TLB/CPU cache space. Context switches = cache misses.
* **Bus contention** - Multiple tasks competing to use buses for IO.



### Numbers

_Originals from Peter Norvig http://norvig.com/21-days.html#answers_

| Operation                           | Time                                   |
| ----------------------------------- | -------------------------------------- |
| execute typical instruction         | 1/1,000,000,000 sec = 1 nanosec        |
| fetch from L1 cache memory          | 0.5 nanosec                            |
| branch misprediction                | 5 nanosec                              |
| fetch from L2 cache memory          | 7 nanosec                              |
| Mutex lock/unlock                   | 25 nanosec                             |
| fetch from main memory              | 100 nanosec                            |
| context switch                      | 5,000 nanosec                          |
| send 2K bytes over 1Gbps network    | 20,000 nanosec                         |
| read 1MB sequentially from memory   | 250,000 nanosec                        |
| fetch from new disk location (seek) | 8,000,000 nanosec                      |
| read 1MB sequentially from disk     | 20,000,000 nanosec                     |
| send packet US to Europe and back   | 150 milliseconds = 150,000,000 nanosec |
--------------------------------------------------------------------------------

## IO

Input/output or IO is sending or receiving data from a peripheral
such as a hard disk or network connection. There are several flavors:

- **Synchronous IO**: a process requests IO and waits for it to complete.
  - Example: unix read/write
- **Asynchronous IO**: a process requests IO and does not wait for it to
  complete. It can do something else, then check back later for a result.
  - Example: epoll/select

**IO is much, much slower than computation** on modern hardware.Programmers must be careful about how they perform IO to fully use compute cores.

## Multitasking

How do we run N tasks on M cores when M < N?

* **Cooperative multitasking** - Tasks run until they terminate or yield control to other tasks
* **Pre-emptive multitasking** - Tasks may be paused to allow other tasks to run

### Scheduling

* **scheduler** Operating system component that decides what tasks to run and when
  * What triggers scheduler?  Hardware interrupts and timers (every ~100ms on linux)
* **context switch** - Switching between tasks.
  * downside: slow (microseconds), cache eviction (TLB/L1 cache)
* **affinity** - Try to keep tasks on the same core to avoid losing TLB and L1 cache

Tradeoffs in picking what to optimize. Common goals:

* **throughput** - Minimize idle core time.
* **latency** - Minimize task completion time.
  * many variations: minimize latency of slowest task, minimize latency of highest priority tasks, etc

Approaches

* **Round Robin** - Run each task for a fixed amount of time.
* **Priority** - Assign tasks priority. Higher priority tasks run first.
  * **Fixed priority** - Tasks assigned fixed priority upfront.
  * **Dynamic priority** - Priority adjusted overtime.
