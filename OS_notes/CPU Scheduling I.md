# Basic Concepts

## CPU-I/O Burst Cycle
Process execution consists of **CPU burst** (executing on CPU) and **I/O burst** (doing I/O wait)
- Generally, there're many short CPU burst and few long CPU burst
- I/O bound process has many short CPU burst
- CPU bound process has a few long CPU burst
## CPU Scheduler
The CPU scheduler selects a process from the ready queue to be executed, its decision may occur when:
Non-preemptive:
1. Switching from running state to waiting state (I/O blocking)
2. termination

Preemptive:
1. Switching from running state to ready state
	+ When a interrupt happens (timer in a time-sharing system fired, system call invoed)
2. Switching from waiting state to ready state
	+ completion of I/O, new candidates
## Preemptive and Nonpreemptive Scheduling
Non-preemptive: Once CPU is allocated to the process, it will holds the CPU until it release either by terminating or switching to waiting state (No interrupts used)

preemptive: scheduling will happens on all cases in [[#CPU Scheduler]], most systems adopt this scheme.
	+ can result in race conditions, we should do data synchronization (e.g. mutex)
## Dispatcher
The dispatcher module belongs to kernel code segment, so it's executed in kernel mode.

Giving controls of the CPU to the process selected by scheduler, the process includes:
1. Switching context (Save the state of old process to PCB)
2. Switching to user mode (jumping to user mode, )
3. Jumping to proper location of the user program and restore the state of new process from *PCB*
BTW, a mode switch can also occur when you want to access hardware by doing a system call, not necessarily needs a context switch.

**dispatch** latency is the time taken for the dispatcher to stop one process and start another execution. (scheduling , interrupt re-enabling and context switch time)
## References
+ https://hackmd.io/@WenHsuanYu/OS-Ch5
---
# Scheduling Criteria
We should maximize:
+ **CPU utilization**: as busy as possible
+ **Throughput**:  number of process that completes its execution per time unit
and minimize:
+ **Turnaround time**: Amount of time to execute a particular process **(submission ~ completion)**
+ **Waiting time**: Amount of time a process waiting in the ready queue. ()
+ **Response time**: submission ~ first response is produced (arrival time ~ first CPU burst moment)
## References
+ https://www.geeksforgeeks.org/operating-systems/interactive-operating-system/
---
# Scheduling Algorithms
## First-Come, First-Served Scheduling
AKA FCFS. Its meaning is trivial, the process with smaller arrival time will be executed first.

**Convoy effect**: If processes with small burst time are arranged (come) after the process with long burst time, the average waiting time will increases.
## Shortest-Job-First Scheduling
There are non-preemptive version and preemptive version. Basically, the CPU is given to the process with **smallest next CPU burst**.
### non-preemptive
once CPU is given to a process, it can't be preempted until completion
### preemptive
Also known as SRJF (Shortest remaining time first), if a new process arrives with shorter (remaining) burst time, a preemption occurs.

Difficulty is, we don't know the next CPU burst time. We commonly used the formula (by taking exponential average) to calculate the next CPU burst.
$$\tau_{n+1}=\alpha t_n+(1-\alpha)\tau_{n}$$
where $t_n$ is the actual length of $n$-th CPU burst , $\tau_n$ is our prediction for $n$-th CPU burst, $\alpha$ is a constant on $[0,1]$.
## Round-Robin Scheduling (RR)
Every process takes turn executing, each round a process executes for a small units of time (called **q (time quantum)**). After the time elapsed, current process is preempted.

The performance:
+ large $q$: FCFS
+ small $q$: the overhead of context switch increases
+ $q$ should be larger than the time of context switch
+ Higher turnaround than SJF, but better response time (typically)s
## Priority Scheduling
Each process has a priority number (smaller integer=higher priority)

**Starvation**: low priority process will never be executed, it can be solved using the **Aging** method, as time elapsed, increase the priority of the process.
## Multilevel Queue Scheduling
The ready queue is separated into several queues (conceptually), we do/maintain several things:
+ Number of queues
+ Schedule the processes in each queue
+ Schedule among queues
+ Decide which queues a process will enter when it needs service. (The priority is static)
Each queue will have its scheduling algorithms, also the scheduling should be done between queues:
+ fixed priority implementation: starvation may occurs
+ solution is using time slices: each queue is assign proper percentages of CPU time
## Multilevel Feedback Queue Scheduling
It's almost the same as multilevel queue scheduling, but the process can demote or upgrade to different queues.
## References
+ https://www.geeksforgeeks.org/dsa/shortest-remaining-time-first-preemptive-sjf-scheduling-algorithm/
+ https://ithelp.ithome.com.tw/m/articles/10277694

---
# Thread Scheduling
When threads are supported, (kernel level) threads are scheduled, not processes. 

User-level threads are managed by a thread library, kernel is unaware of them, and to run them on CPU, they must be *mapped to an associated kernel level thread.* (may use a LWP)

OS further schedule the LWP's kernel thread onto a physical CPU core.
## Contention scope
+ **Process contention scope (PCS)**: the threads library schedule user-level threads on an available LWP to execute.
	+ Scheduling competition is within the process, typically done by programmer.
+ **System contention scope (SCS)**: how kernel decides which kernel-level threads to schedule onto a CPU.
	+ System with one-to-one model only use SCS.
![[Pasted image 20260426205638.png]]
## Pthread scheduling
+ API allows specifying either PCS or SCS during thread creation
+ It can be limited by OS
## references
+ https://ithelp.ithome.com.tw/m/articles/10203786 threads 下
+ https://ithelp.ithome.com.tw/m/articles/10204540 CPU scheduling 中

---

# Multi-Processor Scheduling
Multiprocessor structure:
+ **homogeneous** system: processors are the same in term of their functionality (Multicore CPUs, Multi-threaded cores, NUMA systems)
+ **heterogeneous** system: processors are not identical, and programs need to be compiled as instructions executable on the proper processors.
## Approaches to Multiple-Processor Scheduling
Asymmetric multiprocessing: All scheduling decisions, I/O processing, and other system
activities are handled by a single processor (master server)

Symmetric Multiprocessing (SMP)
- Each processor do scheduling by theirselves.
- Two possible strategies for organizing threads
	- All threads may be in a common ready queue (i.e., having shared data)
	+ Each processor may have its own private queue of threads
## Multicore Processors
Multi-core Processor    
    - faster and consume less power
    - memory stall: 當存取 memory，它會花費很大量時間等待 data 變得可用。(e.g., cache miss)
Multi-threaded multi-core systems
+ 每個 core 指派兩個以上 hardware threads (logical processors)
+ 利用 memory stall 使另一 thread 有進展。

Two levels of scheduling
+ The first level (an operating system) chooses which software thread to run on each hardware thread (logical CPU)
+ The second level specifies how each core decides which hardware thread to run
![[Pasted image 20260426213254.png]]
## Load Balancing
1. Push migration
	+ A specific task periodically checks the load on each processor
	+ If it finds an imbalance, evenly distributes the load by moving (or pushing) threads from overloaded to idle or less-busy processors
2. Pull migration
	+ An idle processor pulls a waiting task from a busy processor
## Processor Affinity
A process has an affinity for the processor on which it is currently
running since the cache of a CPU is usually not shared with the other CPUs.

A thread may be moved from one processor to another to balance loads,
but it loses the contents in the cache of the processor

+ Soft affinity: Attempt to keep a thread running on the same processor, but no
guarantees. So, a process is possible to migrate to other processors.
+ Hard affinity: Allows a process to specify a set of processors it may run on. A process can run ONLY on some of the processors and cannot migrate to others not in the set.
## Heterogeneous Multiprocessing (HMP)
A systems with multiple cores that vary in their clock speeds and power management.
+ The intention behind HMP is to better manage power consumption by assigning tasks to certain cores based upon the specific demands of the task.

ARM big.LITTLE architecture
1. Big cores consume greater energy and therefore should only be used for
short periods of time (e.g., short processing tasks requiring more
power)
2. Little cores use less energy and can therefore be used for longer periods
(such as background tasks)
## References
+ https://hackmd.io/@frakw/S1hFU0Vdt
+ [簡介NUMA架構](https://hackmd.io/@hPMCWajOS-ORQdEEAQ04-w/Hkd1rsonP)
