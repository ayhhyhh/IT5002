## Basic Concept

Program vs. Process:
- Program:
	- Machine Instructions, Data.
	- Exist as a file on the disk.
- Process:
	- Machine Instruction, Data.
	- Context(Registers Content)
	- Exist as instructions and data in memory.
	- Executing.
A single program can produce multiple processes.

## Interrupt

**Interrupt**(sometimes referred to as a **trap**) is a request for the CPU to *interrupt* currently executing code, so that an event can be processed in a timely manner.

The process of Interrupt:
- When a device needs attention from CPU, it trigger a interrupt.
	- Each device is connected to CPU via an input called "Interrupt Request Line" or IRQ line.
	- IRQ Line is checked at the end of **WB stage** in the CPU Execution Cycle.
- If the interrupt is accepted, following things will be executed.
- Current registers(PC, etc.)' content are pushed onto the stack.
- CPU consults an **interrupt vector table** to look for the address of the **interrupt service routine**(ISR, or Interrupt Handler), which is a small bit of code(aka a function) that will tend to the device.
- The address of ISR will be loaded into PC, and CPU starts to execute the interrupt handler to deal with the event.
- When the handler exits, previous PC value and other register content is popped off the stack and restores the scene. Previous process resumes at the interrupted point.
- The CPU asserts the interrupt acknowledge(IA) line to tell the device that its request has been handled.
	- Sometimes CPU will de-assert the IRQ line instead of employing a separate IA line.

## Process

### Process Control Block

When a process is created, a data structure named **Process Control Block**(PCB) is created to maintain information about process. PCB is stored in a table called **Process Table**.

Main content of PCB:
- Process ID (PID)
- Stack Pointer
- Open Files
- Pending Signals
- CPU Usage
- $\cdots$

One Process Table for entire system, one PCB for one process.

### Process State

Basically, A process can be in one of 3 possible states.
- Running:
	- The process is actually being executed on CPU.
- Ready:
	- Process is ready to run but waits for CPU.
- Blocked:
	- Process is waiting for something(except CPU). 
	- e.g. waiting for keyboard input.

![image.png](https://raw.githubusercontent.com/ayhhyhh/IMGbed/master/imgs/202311231755174.png)

### Process Switch

During the execution of a process, CPU needs to maintain a lot of information about it. This is called **Process Context**.
- CPU Register Values.
- Stack Pointers.
- CPU State Word Register.
	- This maintains information about whether previous instruction resulted in an overflow or a zero, whether interrupts are enabled, etc.
	- This is also needed for branch instructions.

When process switching occurs, all information for current process must be saved, and information for the next process must be loaded. This process is know as **context switching**.

- Task A is executing, PC would be pointing at Task A code, SPH/SPL would be pointing at Task A stack, Register R0-R31 contain Task A data.
- Interrupt triggers, PC is placed onto Task A's stack.
- ISR calls `portSAVECONTEXT`. 
- Register R0-R31 and Status Word Registers(SREG) are saved to Task A stack.
- SPH/SPL is saved to Task A PCB.
- Kernel Scheduler selects Task B to run.
- SPH/SPL of Task B is copied from Task B PCB to CPU stack pointer.
- R0-R31 and SREG are loaded from stack to CPU Registers. Now, only Task B's PC value remains on the stack.
- ISR exits, causing PC value popped off onto the PC.
- End Result: Task B resumes execution, with all its data and SREG intact.

### Process Creation

In Python, a process can be created by `fork()` call. The creating process is "Parent" process, while the created process is called a "Child" process.
`subprocess.call` will launch a child process of the shell.

Ultimately, all UNIX processes are children of "init", the main starting process in UNIX.

### Process Termination

- Most resources like open files, etc., will be released and returned to the system.
- PCB is retained in memory
	- For child process.
	- Allow child processes to return results to the parent.
- Parent retrieves the results using a "wait" function call, after which the PCB is released.
	- PCB remains in memory indefinitely.
	- Child process becomes a "zombie process".