# Intro

## What is an operating system?
___
**The OS is a layer of abstraction for handling a PC's resources such as processors, memory, keyboards, displays etc.**

Android is just a layer operating on top of Linux, which runs on top of the bare hardware.

Bare metal programming is difficult as there is no OS and therefore you are programming hardware directly. The OS would usually handle communication which device drivers such as displays. You would also have to manually allocate memory and implement many other solutions to computing problems (e.g. task scheduling, interrupt handling, file systems etc.).

The user interface is not the OS. Not the shell (terminal), nor the GUI. These programmes use the operating system to get work done. 

Most computers have a kernel mode and a user mode. Kernel mode has access to all hardware and can execute any instruction. User mode doesn't allow control of the machine, determine security boundaries, or do I/O.

You can write an email application, but not a clock interrupt handler in user mode.

The user/kernel distinction is blurred in embedded systems (they might not have a kernel mode) or interpreted systems (e.g. Java).

An OS is the code that runs in kernel mode - but not always. They provide application programmers abstractions to use instead of having to directly manage hardware and they manage hardware resources.

Let's say you want to use a hard disk. You would use a programme called a disk driver to interact with it. This programme provides an interface to read and write disk blocks without getting into the details.

But even this level of abstraction is too low for most applications. All OS's therefore provide a file system that allows programmes to create, write, and read files without getting into the hardware details underneath.

One key abstraction is the file. Creating, reading, and editing text at the hardware level would be tough, so we use text files instead.

Hardware is ugly. The Os's job is to make it pretty for the software developer to work with.

From top-down, the OS is a set of abstractions. From bottom-up, the OS deals with hardware management.

The OS deals with which programme gets to use the CPU and when. Also if multiple print jobs are run queued up for a single, printer, which gets printed next. This is a type of resource management known as ***multiplexing***.

Multiplexing can involve the sharing of time, as in the previous examples, or of space. For example, RAM memory can be divided up between multiple programmes.

Early computers only had text-based interfaces for typing in commands. Doug Engelbart from Stanford Research Institute invented the GUI, and his ideas were adopted by researchers at Xerox PARC.

Xerox did not take the GUI seriously but Steve Jobs upon visiting PARC saw the potential and began work on the Lisa, and later the Macintosh. The Mac gained a huge following amongst lay computer users such as graphic designers, photographers and other creatives.

MacOS is based on the Mach microkernel which is UNIX-based.

Microsoft built a successor to MS-DOS influenced strongly by MacOS called Windows, originally operating on top of MS-DOS (acting more like a shell than a true OS). Windows 95 was the first free-standing version of Windows that only used MS-DOS for booting and backwards-compatibility.. Windows NT later replaced 95 as a full 32-bit system, after which came Windows 2000 and then the familiar Windows XP.

After Windows 2000 Microsoft split their OS line into a client and server line. The client line was based on XP and its successors, while the server line produced Windows Server 2003-2019 and now Windows Server vNext. There is also now a third line for embedded systems.

Other OS systems currently in use include Unix-based systems such as Linux (Android), and FreeBSD (macOS is a modified version of FreeBSD).

X Window System (X11) provides a windowing system to Unix-based systems, on top of which users can add a complete GUI such as GNome or KDE to give Unix the look and feel of something like a Mac or Windows machine.

There are also network operating systems as well as distributed operating systems. In network systems users can log into remote machines and copy files from one machine to another (whilst each computer runs its own local OS and has its own local user/users). Such systems are essentially standard operating systems plus features that allow network interface and remote login/file access.

A distributed operating system appears to the user as a uniprocessor system whilst being composed of multiple processors. Users should not be aware of where their programmes are being run or where the files are located. These types of operating systems differ vastly from traditional OS's. They require applications to run on several processors at the same time and thus require more complex processor scheduling algorithms in order to optimise the amount of parallelism. Network communication delays also mean that these algorithms must run with incomplete, outdate, or even incorrect information.


## What's the hardware in a computer?
___

Here we'll run down a simplified model of a computer's hardware.

### 1 - Processors

The 'brain' - the Central Processing Unit (CPU). It fetches instructions from memory and executes them. The basic cycle is to fetch the first instruction from memory, decode it to determine it type and operands, execute it, and then repeat until the programme finishes.

Each CPU has a specific instruction set (a set of instructions that it can execute). An x86 processor cannot execute ARM programmes and vice versa. x86 refers to all Intel processors descended from the 8088 (286, 386, Pentium, i3, i5, i7).

CPUs contain registers to hold key variables and temporary results because accessing memory to get an instruction or data word takes longer than executing an instruction.

The former are general registers which hod variables and temporary results. Most computers also have special registers that are visible to to programmer. One is called the programme counter - which contains the memory address of the next instruction to be fetched. Each time the CPU fetches an instruction, it also updates the programme counter with the address of the next instruction to be fetched.

Another register is the stack pointer, which points to the top of the current stack in memory, which stores things like local variables and input parameters.

There's also the Program Status Word (PSW), which stores information like comparison results, CPU priority, and mode (user or kernel). Normally, user programs can read the PSW but have limited ability to modify it.

The OS is fully aware of all the registers and often stops a running programme to start or restart another one. The OS saves all the registers so that they can be restores when the programme runs later.

There is a distinction between architecture and micro-architecture. Architecture is visible to the software (e.g. instructions and registers). Micro-architecture comprises the implementation of the architecture - that is, how the architecture actually works behind the abstractions. This is not normally visible to the software.

Modern CPUs do not use the simple model of single instruction execution - fetch, decode, execute model. They might have separate units so that multiple instructions can be undergoing the process at once. This is called a pipeline.

A superscalar CPU contains multiple executions units where multiple instructions are fetched at once, decoded, and dumped into a holding buffer to see if there is an instruction it can handle, in which case it executes that instruction. An implication of this is that instructions are often executed out of order. It should be up to the hardware to ensure correct results (sequential implementation would give the same result) - however a lot fo the complexity is foisted onto the OS.

Kernel mode and user mode is switch by a privilege bit/mode bit in the Programme Status Word (PSW). The switch occurs during system calls or interrupts, elevating the privilege level to allow the CPU to execute specific tasks.

The OS runs in kernel mode, other software runs in user mode. You cannot change the privilege level through setting the PSW mode bit outside of kernel mode. To obtain OS services, a user programme must make a system call, which traps it in the kernel and invokes the operating system. In x86-64 processors the trap instruction is `syscall`. This will switch to kernel mode and start the operating system. Once it is done, it returns control to the user programme at the instruction following the system call.

Computers have traps other than the instruction for executing a system call. Most are caused by the hardware to warn of exceptional situations (e.g. zero division or floating point underflow - a result smaller than possible to represent with the given data types number of zeros). Sometime a programme must be terminated with such an error, and other times the error can be ignored (e.g. by setting the underflowed number to 0). If the programme has announced in advance that it want to handle certain kinds of conditions, control can be passed back to the programme to let it deal with the problem.

Moore’s Law observes that the number of transistors on a chip doubles roughly every 18 months, driving more powerful processors. However, as transistors shrink to atomic scales, quantum mechanics will limit further reductions. With more transistors, CPUs initially expanded through superscalar designs and larger caches. Now, they employ multithreading, which allows fast switching between threads to minimize idle time, though not true parallelism. Modern CPUs also feature multicore designs with many independent cores, requiring operating systems to efficiently manage multiple processors. Meanwhile, GPUs, with thousands of small cores, excel at parallel tasks but are challenging to program and less suited for general computing.

### 2 - Memory

The second major component in any computer is memory. Ideally memory should be extremely fast (faster than a CPU is at executing an instruction so that the CPU is not held up by memory), abundantly large, and dirt cheap.

Unfortunately no such current technology satisfies all three goals. Therefore different technologies are used in different layers of the computer.

![[Pasted image 20240915125525.png]]

Top layers have higher speed, smaller capacity, and greater cost per bit than the lower ones (by factors of a billion or more).

Registers are made of the same material as the CPU and thus just as fast - there is no delay in accessing them. The storage capacity is on the oder of 32 x 32 bits on 32 bit systems and 64 x 64 bits on a 64 bit CPU (less than 1KB in both cases). Programmes must manage the registers in software.

Cache memory is the next layer, and is mainly controlled by the hardware. Main memory is divided up into cache lines (typically 64 bytes), with addresses 0 to 63 in cache line 0, 64 to 127 in cache line 1, and so on. The most heavily used cache lines are kept in a high-speed cache located inside or very close to the CPU. When the programme needs to read a memory word, the cache hardware check to see if the line needed is in the cache. If it is, called a **cache hit**, the request is satisfied from the cache and no memory request is sent over the bus to the main memory

Cache hits normally take only a few clock cycles. Cache misses have to go to memory, with a substantial time penalty of tens to hundreds of cycles. It is limited in size due to its high cost. Some machines have two or three layers of cache, each slower and bigger than the last. Cache plays a role bigger than just caching lines of RAM. Caching can be used to improve performance in general - OS's use it all the time, for example by keeping pieces of heavily used files in main memory to avoid having to fetch them from stable storage repeatedly. They are also used to store a disk address as stand-in for a long file path, and also an IP address can be cached once it's been decoded from a URL.

There are several caching questions that pop up:

_1. When to put a new item into the cache._

_2. Which cache line to put the new item in._

_3. Which item to remove from the cache when a slot is needed._

_4. Where to put a newly evicted item in the larger memory._

Most computers have L1 and L2 cache. There is no delay to L1, but a delay of several clock cycles for L2 cache. L1 are typically 32KB each (containing heavily used data words) and L2 can be multiple megabytes, containing recently used memory words.