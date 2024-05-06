# xv6

This is a fork from [xv6](https://github.com/mit-pdos/xv6-public).
This was a project assigned to me in my operating systems course. My main focus was to:

1. Familiarize myself with how operating systems work and how they're built.
2. Familiarize myself with process scheduling
3. Upgrade the primitive scheduler
4. Log the scheduling data to the xv6 terminal

   The new scheduler is the simplified O(1) scheduler. O(1) scheduler was used by the [Linux kernel](http://www.informit.com/articles/article.aspx?p=101760&seqNum=2).
   The policy used by the O(1) scheduler relies on active and expired queues of processes to
   achieve constant scheduling time. Each process in the active queue (AQ) is given a fixed time quantum
   30 ms, after which it is preempted and moved to the expired queue (EQ). Once all the tasks from the
   active queue have exhausted their time quantum (in FIFO order) and have been moved to the expired
   queue, an queue switch takes place. Because the queues are accessed only via pointer, switching them
   is as fast as swapping two pointers. This switch makes the active queue the new empty expired queue,
   while the expired queue becomes the active queue whose time quantum will be reset as 30 ms.
   In the original O(1) scheduler in Linux, each process could be assigned with different priorities. For
   our simplified version, we assume all processes in the active queue have the same priority (low priority)
   except all newly created processes (high priority). Actually, we maintain another queue – called first-
   time queue (FQ) – which maintains all newly-created processes in the FCFS order. Once each new
   process exhausts its time quantum of 10 ms, it will be put into the active queue (AQ). The purpose here is to
   give all newly-created processes the highest priority.
   Therefore, after each timer interrupt is triggered, the scheduler first checks the FQ, if it is not empty, apply RR in the FQ; otherwise, apply RR in the AQ.

xv6 is a re-implementation of Dennis Ritchie's and Ken Thompson's Unix
Version 6 (v6). xv6 loosely follows the structure and style of v6,
but is implemented for a modern x86-based multiprocessor using ANSI C.

## ACKNOWLEDGMENTS

xv6 is inspired by John Lions's Commentary on UNIX 6th Edition (Peer
to Peer Communications; ISBN: 1-57398-013-7; 1st edition (June 14,
2000)). See also http://pdos.csail.mit.edu/6.828/2014/xv6.html, which
provides pointers to on-line resources for v6.

xv6 borrows code from the following sources:
JOS (asm.h, elf.h, mmu.h, bootasm.S, ide.c, console.c, and others)
Plan 9 (entryother.S, mp.h, mp.c, lapic.c)
FreeBSD (ioapic.c)
NetBSD (console.c)

The following people have made contributions:
Russ Cox (context switching, locking)
Cliff Frey (MP)
Xiao Yu (MP)
Nickolai Zeldovich
Austin Clements

In addition, we are grateful for the bug reports and patches contributed by
Silas Boyd-Wickizer, Peter Froehlich, Shivam Handa, Anders Kaseorg, Eddie
Kohler, Yandong Mao, Hitoshi Mitake, Carmi Merimovich, Joel Nider, Greg Price,
Eldar Sehayek, Yongming Shen, Stephen Tu, and Zouchangwei.

The code in the files that constitute xv6 is
Copyright 2006-2014 Frans Kaashoek, Robert Morris, and Russ Cox.

## ERROR REPORTS

If you spot errors or have suggestions for improvement, please send
email to Frans Kaashoek and Robert Morris (kaashoek,rtm@csail.mit.edu).

### BUILDING AND RUNNING XV6

To build xv6 on an x86 ELF machine (like Linux or FreeBSD), run "make".
On non-x86 or non-ELF machines (like OS X, even on x86), you will
need to install a cross-compiler gcc suite capable of producing x86 ELF
binaries. See http://pdos.csail.mit.edu/6.828/2014/tools.html.
Then run "make TOOLPREFIX=i386-jos-elf-".

To run xv6, install the QEMU PC simulators. To run in QEMU, run "make qemu".

To create a typeset version of the code, run "make xv6.pdf". This
requires the "mpage" utility. See http://www.mesa.nl/pub/mpage/.

In my case, I pulled the project as an image from docker hub given by my professor. I ran a docker container based on that image.
