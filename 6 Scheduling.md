
**Scheduling** is the action of assigning resources to perform tasks.(In this lecture, only refer to CPU resource). Scheduler is invoked at following points shown below.

![image.png](https://raw.githubusercontent.com/ayhhyhh/IMGbed/master/imgs/202311232119876.png)

Scheduling Policy depends on the type of multitasking environment.

Multitaskers:
- Batch Processing:
	- Not actually multitasking, one process runs at a time to completion.
	- **First Come First Serve**, **Shortest Job First**.
- Co-operative Multitasking:
	- Aka non-preemptive multitasking. Currently running processes won't be suspended by the scheduler.
	- Processes must volunteer to give up CPU.
	- **Round Robin** with Voluntary Scheduling.
- Pre-emptive Multitasking:
	- Scheduler can forcefully suspend currently running processes.
	- **Round Robin** with Timer, **Shortest Remaining Time**
- Real-Time Multitasking:
	- Processes have a fixed deadline that must be met.
	- Two type: Hard Real-Time, Soft Real-Time(less strict)
	- (Not Covered) Rate Monotonic Scheduling, Earliest Deadline First Scheduling.

Multiple policies can be implemented and deployed on the same machine at the same time. e.g.:
- For high priority tasks: RR policy.
- For low priority tasks: FCFS policy.

## Scheduling Policy

### Fixed Priority Policy

This is a simple policy which can be used across any type of multitaskers.

- Each task is assigned a fixed priority by programmer.
- Tasks are in a priority queue according to the priority number.
- For Batch, Co-operative:
	- Task with highest priority is picked to run next.
- For Pre-emptive, Real-Time:
	- Current task is suspended and higher priority task is run.

### First Come First Serve

- Queue
- Particularly suited for batch systems
- For interactive system:
	- Jobs removed for running are put back into the back of the queue.(Round-Robin)
- **Free from Starvation**.

### Shortest Job First

- For batch system.
- Processes are ordered by total CPU time(estimated).
- Smallest average(total) waiting time.
- Potential for starvation. 

### Round Robin with Voluntary Scheduling

- For co-operative scheduling system.
- Process calls a special "yield" function
- Invokes the scheduler to suspend the process.
- Starts another process at the front of the queue.
- Add the suspended process to the back of the queue.

### Shortest Remaining Time

- For pre-emptive system.
- Like SJF, processes are ordered according to remaining CPU time left.

### Round Robin with Timer

- For pre-emptive system.
- Each process is assigned a fixed time slot $c_i$.
- After executing for time $c_i$ , scheduler is invoked and begin scheduling on a round-robin basis.

## Scheduling in Linux

pass