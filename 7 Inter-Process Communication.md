
**Race Condition**: occurs when two computer program processes, or threads, attempt to access the same resource at the same time.
- Global Variables.
- Memory Locations.
- Hardware Registers.(Configuration Registers)
- Files.

**Critical Section**: the segment of code where processes access shared resources.

**Mutual Exclusion**: aka "mutex", prevent two processes from accessing share resources at the same time.

4 rules preventing race conditions:
- No two processes can simultaneously be in their critical section.
- No assumption may be made about speeds or num of CPUs.
- No process outside its critical section can block other processes.
- No process should wait forever to enter its critical section.

## Implement Mutual Exclusion

### Disable Interrupt

Process Switching depends on Interrupt. Once a process disables interrupt and enters its critical section, NO other process can get run unless the process exits from critical section and enables interrupt.

Problems:
- Carelessly disabling interrupt can cause the entire system to grind to a halt.
- Only works on single-core processor.(Violates Rules 2)

### Test and Set Lock

Normally set a global variable "lock" won't work because it is still a shared resource.

Test and Set Lock(TSL) is an atomic operation which is guaranteed by hardware and won't be interrupted during its execution.

```
Enter:
	TSL Reg, LOCK # Copy lock to register and set lock to 1, this step is atomic
	CMP Reg, $0 
	JNE Enter
	RET
Leave:
	MOVE LOCK, $0
	RET
```

### Semaphore

A semaphore is a special lock variable that counts the num of wake-ups saved for future use. Operations on semaphore is ATOMIC.
- DOWN, TAKE, PEND, or P:
	- If $semaphore > 0$, it is decremented, DOWN operation returns.
	- If $semaphore = 0$, DOWN operation blocks.
- UP, POST, GIVE, or V:
	- If there are processes blocking on a DOWN, one of them is selected and woken up.
	- Else, semaphore is incremented and returns.

### Monitor

Monitor is a special data structure that **ONLY** one process can be active in the monitor at any point in time.(in C++ or JAVA, guaranteed by complier)
- If any other process tries to call a method within the monitor when there has been a process calling monitor method(no matter it is running or suspended), it will be blocked until the other process exits the monitor.

Condition Variables: 
- Like semaphore:
- One process **WAIT**s on a condition variable and blocks until
- Another process **SIGNAL**s on the same condition variable, unblocking the **WAIT**ing processes.

Problem: When a process encounters a WAIT and is blocked, another process is allowed to enter the monitor, and put a SIGNAL to wake up the previous process. Potentially, there are two processes in the monitor at the same time.
- SIGNALed process.
- SIGNALing process.
Resolve:
- Require the signaler exits immediately.
- Suspend the signaler.
- Suspend the signaled process until signaler exits.

### Barrier

**Barrier** is a special form of synchronization mechanism that works with groups of processes rather than single process. Processes can only pass barrier only when all related processes get barrier.
- Example application: Computing fluid motion.

![image.png](https://raw.githubusercontent.com/ayhhyhh/IMGbed/master/imgs/202311242133552.png)

## Producer and Consumer Problem

### Semaphore

```C
#define N 100
typedef int semaphore;
semaphore mutex = 1;
semaphore empty = N;
semaphore full = 0;

void producer(void){
	while(1){
		item = produce_item();
		down(&empty);
		down(&mutex);
		insert_item(item);
		up(&mutex);
		up(&full);
	}
}
void consumer(void){
	while(1){
		down(&full);
		down(&mutex);
		item = remove_item();
		up(&mutex);
		up(&empty);
		consume(item)
	}
}
```

### Monitor

```
monitor ProducerConsumer
	condition full, empty;
	integer count = 0;
	function insert(item):
	begin
		if count = N then wait(full);
		insert_item(item);
		count := count + 1;
		if count = 1 then signal(empty);
	end;
	function remove():item:
	begin
		if count = 0 then wait(empty);
		item = remove_item();
		count := count - 1;
		if count = N - 1 then signal(full);
	end;

procedure producer:
begin
	while 1 do:
	begin
		item = produce_item();
		ProducerConsumer.insert(item);
	end
end;

procedure consumer:
begin
	while 1 do:
	begin
		item = ProducerConsumer.remove();
		consume_item(item);
	end
end;
```

## Dead Lock

**Deadlock** is an situation in which no member of some group of entities can proceed because each waits for mutual exclusive shared resources which has been requested and controlled by other members.

Deadlock Necessary Condition:
- Mutual Exclusion.
- Hold and wait.
- Circular Wait.

Dealing with Deadlock:
- Detection and Recovery:
	- Allow deadlock to happen and eliminate it.
- Avoidance
	- Dynamic.
	- Check and disallow allocations that might lead to dead locks.
	- Banker's Algorithm.
- Prevention
	- Static. Break one or more necessary conditions of deadlock
	- Eliminate mutual exclusion.
		- Spooling, makes I/O devices shareable.
		- Not possible in most cases.
	- Eliminate hold-and-wait.
		- Request all resources at once.
		- Release all resources before a new request.
		- Release all resources if current request blocks.
	- Eliminate circular wait.
		- Order all resources.
		- Process must request in **ascending order**.

Deadlock Special Case: **Priority Inversion**
Tip: CPU is also a mutual exclusive resource.
![image.png](https://raw.githubusercontent.com/ayhhyhh/IMGbed/master/imgs/202311242213187.png)

### Deadlock in Producer and Consumer Problem

If swap two semaphore, it will result in a deadlock potentially.

```
void producer(void){
	while(1){
		item = produce_item();
		down(&mutex); // swapped
		down(&empty); // swapped
		insert_item(item);
		up(&mutex);
		up(&full);
	}
}
void consumer(void){
	while(1){
		down(&mutex); //swapped
		down(&full); //swapped
		item = remove_item();
		up(&mutex);
		up(&empty);
		consume(item)
	}
}
```
