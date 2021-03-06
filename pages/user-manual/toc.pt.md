---
layout: page
lang: pt
permalink: /pt/user-manual/
title: Manual do usuário
author: Liviu Ionescu

date: 2016-06-28 19:39:00 +0300

---

Note: The User's Manual is currently work in progress.

## RTOS

### [Começando com μOS++]({{ site.baseurl }}/{{ page.lang }}/user-manual/getting-started/)
  * [Overview]({{ site.baseurl }}/{{ page.lang }}/user-manual/getting-started/#overview)
    * [Multiple APIs]({{ site.baseurl }}/{{ page.lang }}/user-manual/getting-started/#multiple-apis)
  * [The `os_main()` and the main thread]({{ site.baseurl }}/{{ page.lang }}/user-manual/getting-started/#the-osmain-and-the-main-thread)
  * [Multiple thread applications]({{ site.baseurl }}/{{ page.lang }}/user-manual/getting-started/#multiple-thread-applications)

### [Conceitos básicos]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/)

* [Soft vs hard real-time systems]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#soft-vs-hard-real-time-systems)
* [Superloop (foreground/background) applications]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#superloop-foregroundbackground-applications)
* [Multi-tasking]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#multi-tasking)
  * [Tasks]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#tasks)
  * [Why multiple threads?]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#why-multiple-threads)
  * [Threads & processes]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#threads--processes)
  * [Blocking I/O]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#blocking-io)
  * [The idle thread]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#the-idle-thread)
  * [Sleep modes and power savings]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#sleep-modes-and-power-savings)
  * [Context switching]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#context-switching)
  * [Thread stacks]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#thread-stacks)
  * [Cooperative vs preemptive multi-tasking]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#cooperative-vs-preemptive-multi-tasking)
  * [Thread interruption/cancellation]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#thread-interruption-cancellation)
* [Scheduling]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#scheduling)
  * [Thread states]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#thread-states)
  * [The READY list]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#the-ready-list)
  * [Scheduling algorithms]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#scheduling-algorithms)
  * [Round-robin vs priority scheduling]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#round-robin-vs-priority-scheduling)
  * [Selecting thread priorities]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#selecting-thread-priorities)
  * [Priority inversion / priority inheritance]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#priority-inversion--priority-inheritance)
* [Communicating between threads and/or ISRs]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#communicating-between-threads-andor-isrs)
  * [Periodic polling vs event waiting]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#periodic-polling-vs-event-waiting)
  * [Passing messages]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#passing-messages)
  * [Event flags]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#event-flags)
  * [Semaphores]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#semaphores)
* [Managing common resources]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#managing-common-resources)
  * [Disable/enable interrupts]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#disable-enable-interrupts)
  * [Lock/unlock the scheduler]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#lock-unlock-the-scheduler)
  * [Counting semaphores]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#counting-semaphores)
  * [Mutual exclusion (mutex)]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#mutual-exclusion-mutex)
  * [Should a semaphore or a mutex be used?]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#should-a-semaphore-or-a-mutex-be-used)
  * [Deadlock (or deadly embrace)]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#deadlock-or-deadly-embrace)
* [Statically vs. dynamically allocated objects]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#statically-vs-dynamically-allocated-objects)
  * [The system allocator]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#the-system-allocator)
  * [Fragmentation]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#fragmentation)
* [Real-time clock]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#real-time-clock)
* [Terms to use with caution]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#terms-to-use-with-caution)
  * [Kernel]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#kernel)
  * [Tasks vs threads]({{ site.baseurl }}/{{ page.lang }}/user-manual/basic-concepts/#tasks-vs-threads)

### [Features]({{ site.baseurl }}/{{ page.lang }}/user-manual/features/)

### [Threads]({{ site.baseurl }}/{{ page.lang }}/user-manual/threads/)
  * [Overview]({{ site.baseurl }}/{{ page.lang }}/user-manual/threads/#overview)
  * [Thread functions]({{ site.baseurl }}/{{ page.lang }}/user-manual/threads/#thread-functions)
  * [Thread priorities]({{ site.baseurl }}/{{ page.lang }}/user-manual/threads/#thread-priorities)
  * [Creating threads]({{ site.baseurl }}/{{ page.lang }}/user-manual/threads/#creating-threads)
  * [Changing thread priorities]({{ site.baseurl }}/{{ page.lang }}/user-manual/threads/#changing-thread-priorities)
  * [Other thread functions]({{ site.baseurl }}/{{ page.lang }}/user-manual/threads/#other-thread-functions)
  * [Destroying threads]({{ site.baseurl }}/{{ page.lang }}/user-manual/threads/#destroying-threads)
  * [The current thread]({{ site.baseurl }}/{{ page.lang }}/user-manual/threads/#the-current-thread)
  * [Thread states]({{ site.baseurl }}/{{ page.lang }}/user-manual/threads/#thread-states)
  * [The thread stack]({{ site.baseurl }}/{{ page.lang }}/user-manual/threads/#the-thread-stack)
  * [The idle thread]({{ site.baseurl }}/{{ page.lang }}/user-manual/threads/#the-idle-thread)
  * [The main thread]({{ site.baseurl }}/{{ page.lang }}/user-manual/threads/#the-main-thread)

### [Thread event flags]({{ site.baseurl }}/{{ page.lang }}/user-manual/thread-event-flags/)
  * [Overview]({{ site.baseurl }}/{{ page.lang }}/user-manual/thread-event-flags/#overview)
  * [Raising thread event flags]({{ site.baseurl }}/{{ page.lang }}/user-manual/thread-event-flags/#raising-thread-event-flags)
  * [Waiting for thread event flags]({{ site.baseurl }}/{{ page.lang }}/user-manual/thread-event-flags/#waiting-for-thread-event-flags)
  * [Other thread event flags functions]({{ site.baseurl }}/{{ page.lang }}/user-manual/thread-event-flags/#other-thread-event-flags-functions)

### [Semaphores]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/)
  * [Overview]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#overview)
  * [Semaphore types]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#semaphore-types)
    * [Binary semaphores]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#binary-semaphores)
    * [Counting semaphores]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#counting-semaphores)
  * [Creating semaphores]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#creating-semaphores)
  * [Posting to semaphores]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#posting-to-semaphores)
    * [Posting to semaphores from ISRs]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#posting-to-semaphores-from-isrs)
  * [Waiting on semaphores]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#waiting-on-semaphores)
  * [Multiple threads waiting on a semaphore]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#multiple-threads-waiting-on-a-semaphore)
  * [Other semaphore functions]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#other-semaphore-functions)
    * [Getting the semaphore name]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#getting-the-semaphore-name)
    * [Getting the semaphore value]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#getting-the-semaphore-value)
    * [Getting the semaphore maximum value]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#getting-the-semaphore-maximum-value)
    * [Getting the semaphore initial value]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#getting-the-semaphore-initial-value)
  * [Destroying semaphores]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#destroying-semaphores)
  * [Using semaphores for resource management]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#using-semaphores-for-resource-management)
  * [Unilateral rendezvous]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#unilateral-rendezvous)
  * [Semaphore pitfalls]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#semaphore-pitfalls)
    * [Unbalanced wait()/post()]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#unbalanced-waitpost)
    * [Recursive deadlock]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#recursive-deadlock)
    * [Thread-termination deadlock]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#thread-termination-deadlock)
    * [Priority inversion]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#priority-inversion)
    * [Wrong initialisation]({{ site.baseurl }}/{{ page.lang }}/user-manual/semaphores/#wrong-initialisation)

### [Event flags]({{ site.baseurl }}/{{ page.lang }}/user-manual/event-flags/)
  * [Overview]({{ site.baseurl }}/{{ page.lang }}/user-manual/event-flags/#overview)
  * [Creating event flags]({{ site.baseurl }}/{{ page.lang }}/user-manual/event-flags/#creating-event-flags)
  * [Raising event flags]({{ site.baseurl }}/{{ page.lang }}/user-manual/event-flags/#raising-event-flags)
    * [Raising event flags from ISRs]({{ site.baseurl }}/{{ page.lang }}/user-manual/event-flags/#raising-event-flags-from-isrs)
  * [Waiting for event flags]({{ site.baseurl }}/{{ page.lang }}/user-manual/event-flags/#waiting-for-event-flags)
  * [Multiple threads waiting on event flags]({{ site.baseurl }}/{{ page.lang }}/user-manual/event-flags/#multiple-threads-waiting-on-event-flags)
  * [Other event flags functions]({{ site.baseurl }}/{{ page.lang }}/user-manual/event-flags/#other-event-flags-functions)
    * [Getting the event flags name]({{ site.baseurl }}/{{ page.lang }}/user-manual/event-flags/#getting-the-event-flags-name)
    * [Getting individual flags]({{ site.baseurl }}/{{ page.lang }}/user-manual/event-flags/#getting-individual-flags)
    * [Clearing individual flags]({{ site.baseurl }}/{{ page.lang }}/user-manual/event-flags/#clearing-individual-flags)
  * [Destroying event flags]({{ site.baseurl }}/{{ page.lang }}/user-manual/event-flags/#destroying-event-flags)

### [Mutexes]({{ site.baseurl }}/{{ page.lang }}/user-manual/mutexes/)
  * [Overview]({{ site.baseurl }}/{{ page.lang }}/user-manual/mutexes/#overview)
    * Normal mutexes
    * Recursive mutexes
  * Creating mutexes
  * Acquiring a mutex
  * Releasing a mutex
  * Multiple threads waiting on a mutex
  * Other mutex functions
  * Destroying mutexes
  * Priority inversion
  * Priority inheritance
  * Deadlock (or deadly embrace)

### Condition variables
  * Overview
  * TODO


### Message queues
  * Overview
  * Queue storage
  * Message priorities
  * Creating queues
  * Sending messages to queues
    * Sending messages from ISRs
  * Receiving messages from queues
  * Other message queue functions
  * Multiple threads waiting on message queues
  * Destroying queues

### Fixed size memory pools
  * Overview
  * Pool storage
  * Creating memory pools
  * Getting a memory block
    * Getting a memory block from ISRs
  * Returning a memory block
  * Other memory pools functions
  * Multiple threads waiting on memory pools
  * Destroying memory pools

### Software timers
  * Overview
  * Timer functions
  * Creating timers
  * Start a timer
  * Stop a timer
  * Other timer functions
  * Destroying timers

### Clocks
  * Overview
  * The system clock
  * The real time clock
  * The high resolution clock

### Interrupts & critical sections
  * Overview
  * High/low priority interrupts
  * Interrupt nesting
  * Critical sections
  * Uncritical sections

### Memory allocators
  * TODO

### The scheduler
  * Scheduling points
  * The ready list
  * The scheduling algorithm
  * Waiting lists
  * The idle thread

### Run-time statistics
  * Per-thread number of context switches
  * Per-thread number of CPU clocks used

### Iterating threads

### Credits

As the saying goes, _"Books are written from books, and software from software"_. As such, this manual too did not appear from nothing, but was influenced by the following manuals:

- _"Using the FreeRTOS Real Time Kernel"_, by Richard Barry
- _"µC/OC-III The Real-Time Kernel - User's Manual"_, by Micriµm
- _"embOS & embOS-MPU - Real-Time Operating System - CPU-independent - User & Reference Guide"_, by SEGGER

Other links:

- [The Feabhas Blog](https://blog.feabhas.com/category/rtos/)

Many thanks for their impressive work and for providing the inspiration.
