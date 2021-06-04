# ADB Final

## 1st Week

**Keywords: Disk access time, Memory access time, SSD, Moore's law, Joy's law, Database archtecture**

![avatar](./pic/disk.png)

### Disk Access time

Disk access time(HDD) = seek time + rotational time + transfer length/bandwitdth  
Disk access time(SSD) = transfer length/bandwitdth  

### Laws

Moore's law(memory chip): Chip capacity = $2^{\frac{2*(year-1970)}{3}}KB/chip$  
Joy's law(processor): Processor performance = $2^{(year-1984)/2} mips$  

### Memory access time

Effective memory access time: EA = H*C+(1-H)*M  
Hit ratio = $\frac{references\ satisfied\ by\ cache}{total references}$  
H: hit ratio, C: cache access time, M: memory access time

### Types of database systems

- Simple file: plain text file
- RDBS: collection of tables
- Object oriented: Datastored in Objects
- No-SQL: Non relational database

### Database architectures

- Centralized: Data stored in one place
- Distributed:  Data distributed across several nodes, can be in different locations
- WWW: Stored all over the world, several owners of the data
- Grid: Like distributed, but each node manages own resource; system doesn’t act as a single unit.
- P2P: Like grid, but nodes can join and leave network at will (unlike Grid)
- Cloud: Generalization of grid, but resources are accessed on demand.

<hr>

## 2nd 3rd Week

**Keywords:Storage area networks(SAN)**
![avatar](./pic/SAN.png)

### ACID  

- Atomicity: A transaction’s changes to the state (Database) are atomic implying either all actions happen or none happen.
- Consistency: Transaction are a correct transformation of the state. Actions taken as a whole by a transaction do not violate the integrity of the application state. That is we are assuming transactions are correct programs. Concurrent execution of transactions does not violate consistency of data. Data is in a ‘consistent’ state when a transaction starts and when it ends – in other words, any data written to the database must be valid according to all defined rules
- Isolation: Transaction are executed as if it is the only one in the system. Even when several transactions are executed simultaneously, it appears to each transaction T that others executed either happen before T or after T but not at the same time.
- Durability: State changes committed by a transaction survive failures. The system should tolerate system failures and any committed updates should not be lost.

### Fault tolerance by RAID

- RAID 0 (block level striping)
  - striping at block level
  - throughput double and MTTF(mean time to failure) half
  - MTTF/2
- RAID 1 (mirroring)
  - higher read throughput
  - lower write throughput
  - half storage utilization
  - $MTTF^2$
- RAID 2 (bit level striping)
  - similar to raid 0, striping 1 bit level
  - MTTF/2
- RAID 3 (byte level striping)
  - byte level striping
  - transfer rate > RAID 0
  - $MTTF^2/3$
  - parity
- RAID 4 (block level striping)
  - similar to RAID 3
  - Dedicated disk for parity blocks
  - higher throughput but very slow writes
  - $MTTF^2/3$
- RAID 5 (block level striping)
  - similar to RAID 4, parity blocks are also striped
  - Provides higher throughput but slower writes but better than RAID 4 as Parity bits are distributed among all disks and the number of write operations on average equal among all 3 disks.
  - $MTTF^2/3$
- RAID 6 (block level striping)
  - similar to RAID 5 two parity blocks used
  - $MTTF^3/10$(two disk breakdown tolerance)
  
![raid](pic/RAID.png)

### Fault tolerance by voting

- **failvote**: stop if there are no majority agreement
- **failfast**: uses the majority of the available modules.
- **Supermodule**: Naturally, a system with multiple hard disk drives is expected to function with only one working disk (use voting when multiple disks are working/available, but still work even when only one is available)

十个盘里坏了5个failvote就罢工，坏9个failfast罢工,supermodel继续工作

### Fault tolerance with repair

**With repair of modules**: the faulty equipment is repaired with an average time of MTTR (mean time to repair) as soon as a fault is detected.  

Probability of a particular module is not available = $MTTR/(MTTF+MTTR) = MTTR/MTTF$ (MTTF >> MTTR)  

Probability that (n-1) modules are unavailable:$P_{n-1} = (\frac{MTTR}{MTTF})^{n-1}$

Probability that a particular module fails: $P_{i} = \frac{1}{MTTF}$

Probability that the system fails with a particular module failing last: $P_{n-1}*P_{i}$

Probability that a supermodule fails due to any one of the n modules failing last, when other (n-1) modules are unavailable $n*P_{n-1}*P_{i}$

### Old master - new master technique 

- record all updates (transactions) to be performed in a separate file
- at night (usually) produce a separate new (next day) master using the old (previous day’s) master and the batch updates (transactions).
- Not online processing, transaction failure is not notified until next day
  
### Software reliability

#### Main distinction between software and hardware reliability

- Hardware reliability requires tolerating component failures.
- Software reliability requires tolerating design and coding faults.
- The distinction between Hardware and Software is becoming less as most hardware units have substantial amount of software components

#### Improvement

1. N-version programming
   - use n programs that run in parallel, take majority vote for each answer
   - the diversity of design and coding can mask many failures
2. Software reliability as 'transaction' (Abort and restart)
   - Write a program as an ACID state transformation with consistency checks. If the consistency checks are not met (fault), abort the transaction and restart
   - A recent consistent system state is restored after restart
   - Restarting an application without proper recovery (repair) can make the systems very unreliable
   - **Bohrbugs**: deterministic bugs, relatively easy to handle
   - **Heisenbugs**: software errors that only appear occasionally
3. Process pairing
   - Primary process does all the work until it fails. The second process (backup) takes over the primary and continues. 
   - Primary needs to tell on a regular basis that it is alive and also transmit its state to the secondary.
   - three ways to takeover
     - Checkpoint-restart: The primary records its state on a second storage module. At takeover the secondary starts reading the state of the primary from the storage and resumes the application. (全记录)
     - Checkpoint messages: The primary sends its state changes as messages to the backup. At takeover the backup gets its current state from the most recent checkpoint message.（只记录改变）
     - Persistent: backup restarts in the null state and lets Transaction mechanism to clean up all uncommitted transactions. This is the approach taken by the most Database Systems.(现在最常用的)

<hr>

## 4th 5th Week

**Keywords: Communication, Transaction**

### Communication reliability

![communication](pic/communication.png)

### Improving system’s reliability

- Improving Hardware reliability that is CPUs, Memory and Storage Units. This can be done by employing lot of redundancy
- Software reliability can be improved by employing process-pairing or transaction-based recovery.
- Communication Systems reliability not only need hardware redundancy but also need to guaranty transmission and reception of messages using stable storage and acknowledgements

### Database transaction

Transaction: a unit of work in a database

- A transaction can have any number and type of operations in it
- Transactions ideally have four properties, commonly known as ACID properties

#### Duplex write

- Each block of data is written in two places sequentially
- If one of the writes fail, system can issue another write
- Each block is associated with a version number. The block with the latest version number contains the most recent data.
- While reading - we can determine error of a disk block by its Cyclic Redundancy Check (CRC).
- It always guarantees at least one block has consistent data.

#### Logged write

- Similar to duplex write, except one of the writes goes to a log. This method is very efficient if the changes to a block are small.

![updatingdisk](pic/Transaction.png)

#### Cyclic Redundancy Check (CRC)

To compute an n-bit binary CRC

1. Add n zero bits as ‘padding’ to the right of the input bits.  
2. Compute the (n + 1)-bit pattern representing the CRC's divisor (called a "polynomial")
3. Position the (n + 1)-bit pattern representing the CRC's divisor underneath the left-hand end of the input bits.
4. The algorithm acts on the bits directly above the divisor in each step.
  
![crc](pic/crc.png)

Checking validity with CRC

![checkcrc](pic/checkcrc.png)

##### An error detection algorithm

1. A polynomial needs to be specified
2. A sequence of bitwise exclusive-or (XOR) operation needs to be performed
3. The final CRC value needs to be stored for each data block (or the data unit on which CRC is performed)
4. Data correctness can be checked with CRC: a) its corresponding CRC value is retrieved.b) A sequence of bitwise XOR operation needs to be performed to find out the correctness of data

### Different transaction

- **Flat transaction**：Any failure of transaction requires lot of unnecessary computation. (一旦出错所有的工作都rollback)
  - Everything inside BEGIN WORK and COMMIT WORK is at the same level; that is, the transaction will either survive together with everything else (commit), or it will be rolled back with everything else (abort)

- **Flat transaction with save points**
- **nested transaction**
  - Commit rule
    - A subtransaction can either commit or abort, however, **commit cannot take place unless the parent itself commits**.
    - Subtransactions have A, C, and I properties but not D property unless all its ancestors commit.
    - Commit of a sub transaction makes its results available only to its parents.
  - Roll back rule
    - If a subtransaction rolls back, all its children are forced to roll back.(父退回时子退回)
  - Visibility Rules
    - Changes made by a subtransaction are visible to the parent only when the subtransaction commits. All objects of parent are visible to its children. Implication of this is that the **parent should not modify objects while children are accessing them**. This is not a problem as parent does not run in parallel with its children.(子提交的父可见，父的所有子可见，子占用时父不占)

### Transaction Processing Monitor (TP monitor)

The main function of a TP monitor is to integrate other system components and manage resources.

- TP monitors manage the transfer of data between clients and servers
- Breaks down applications or code into transactions and ensures that all databases are updated properly
- It also takes appropriate actions if any error occurs

#### TP monitor services

- Heterogeneity: If the application needs access to different DB systems, local ACID properties of individual DB systems is not sufficient. Local TP monitor needs to interact with other TP monitors to ensure the overall ACID property. A form of 2 phase commit protocol must be employed for this purpose.
- Control communication: If the application communicates with other remote processes, the local TP monitor should maintain the communication status among the processes to be able to recover from a crash.
- Terminal management: Since many terminals run client software, the TP monitor should provide appropriate ACID property between the client and the server processes.
- Presentation service: this is similar to terminal management in the sense it has to deal with different presentation (user interface) software -- e.g. X-windows
- Context management: E.g. maintaining the sessions etc.
- Start/Restart: There is no difference between start and restart in TP based system.

#### TP monitor structure

![tpstructure](pic/tpstructure.png)

<hr>

## 6th Week

### Concurrency

Impose exclusive access for correct execution

#### Concurrency Control

To resolve conflicts and preserve database consistency

- [**Dekker's algorithm**](https://www.youtube.com/watch?v=MqnpIwN7dz0) (using code)
  - needs almost no hardware support, needs atomic reads and writes to main memory, That is exclusive access of one time cycle of memory access time!
  - the code is very complicated to implement for more than two transactions
  - harder to understand the algorithm for more than two transactions
  - takes lot of storage space
  - efficient for low lock contention
  - uses busy waiting
- **OS supported primitives**
  - need no special hardware
  - independent of number of processes
  - machine independent
  - through an interrupt call, the lock request is passed to the OS
  - are very expensive (several hundreds to thousands of instructions need to be executed to save context of the requesting process.)
  - do not use busy waiting and therefore more effective
- **Spin locks**(访问之前查看上锁状态)
  - using atomic lock/unlock instructions such as **test and set** or **compare and swap**
  - need hardware support – should be able to lock bus (communication channel between CPU and memory + any other devices) for two memory cycles (one for reading and one for writing). During this time no other devices’ access is allowed to this memory location.
  - algorithm does not depend on number of processes
  - are very efficient for low lock contentionsv (all DB systems use them)
  - use busy waiting

#### Atomic operation

- Test and set: 查看始否上锁，没上锁就上锁返回true，上了锁返回false
- Compare and swap: 查看始否和旧值相等，相等则赋予新值并返回true，否则重置旧值返回false

### Semaphore

Semaphores derive from the corresponding mechanism used for trains: a train may proceed through a section of track only if the semaphore is clear. Once the train passes, the semaphore is set until the train exits that section of track.

Computer semaphores have a **get()** routine that acquires the semaphore, perhaps waiting until it is free and a dual **give()** routine that returns the semaphore to the free state, perhaps waking up a waiting process.

Semaphores are very simple locks; indeed, they are used to implement
**general-purpose locks**.

Spin locks are the most commonly used locks

#### Implementation of Exclusive mode Semaphore(说白了就是链表排队)

1. Pointer to a queue of processes
2. If the semaphore is busy but there are no waiters, the pointer is the address of the process that owns the semaphore.
3. If some processes are waiting, the semaphore points to a linked list of waiting processes. The process owning the semaphore is at the end of this list.
4. After usage, the owner process wakes up the oldest process in the queue (first in, first out scheduler)

#### Convoy avoiding semaphore

- The previous implementation may result a long list of waiting processes, called convoy
- To avoid convoys, a process may simply free the semaphore (set the queue to null) and then wake up every process in the list after usage.
- In that case, each of those processes will have to re-execute the routine for acquiring semaphore.(重新排队)

### Deadlocks(等对方解锁)

In a deadlock, each process in the deadlock is waiting for another member to release the resources it wants.

#### Solutions

- Have enough resources so that no waiting occurs(不现实)
- Pre-declare all necessary resources and allocate in a single request.(不现实)
- Linearly order the resources and request of resources should follow this order.This type of allocation guarantees no cyclic dependencies among the transactions.
- Periodically check the resource dependency graph for cycles. If a cycle exists - rollback (i.e., terminate) one or more transaction to eliminate cycles (deadlocks). The chosen transactions should be cheap.
- Allow waiting for a maximum time on a lock then force Rollback.
- Many distributed database systems maintain only local dependency graphs and use time outs for global deadlocks.

## 7th Week

### Isolation

Multiple concurrently running transactions may cause conflicts, allow concurrent runs as much as possible for a better performance, while avoiding conflicts as much as possible.  

Isolation ensures that concurrent transactions leaves the database in the same state as if the transactions were executed separately.  

Isolation guarantees consistency, provided each transaction itself is consistent.

When dependency graph has cycles then there is a violation of isolation and a possibility of inconsistency.

#### Conflicts

- write-write (Lost update): 覆盖其他transaction的update
- read-write (Unrepeatable read): 之前读取的被修改导致再次读取结果不一致
- write-read (Dirty Read): 未提交的被读取

#### Dependency relation

In many situation inputs and outputs may be state dependant/not known in prior

H1 = <(T1,R,O1), (T2, W, O5), (T1,W,O3), (T3,W,O1), (T5,R,O3), (T3,W,O2), (T5,R,O4), (T4,R,O2), (T6,W,O4)>  
DEP(H1) = {<T1,O1,T3>, <T1,O3,T5>, <T3,O2,T4>, <T5,O4,T6> }  
H2 = <(T1,R,O1), (T3,W,O1), (T3,W,O2),(T4,R,O2),(T1,W,O3), (T2, W, O5), (T5,R,O3), (T5,R,O4), (T6,W,O4)>  
DEP(H2) = {<T1,O1,T3>, <T1,O3,T5>, <T3,O2,T4>, <T5,O4,T6> }  

DEP(H1)=DEP(H2)

#### Isolated history

- A serial history = an isolated history.
- A serial history is history that is resulted as a consequence of running transactions sequentially one by one.
- N transactions can result in a maximum of N! serial histories.
- Wormhole theorem: A history is isolated if and only if it has no wormholes.

#### Wormhole transaction

$T' \in Before(T) \cap After(T)$  
Exist a cycle in the dependency graph of the history. Presence of a wormhole transaction implies it is not isolated

#### Isolation Theorems

- A transactions is a sequence of READ, WRITE, SLOCK, XLOCK actions on objects ending with COMMIT or ROLLBACK.
- A transaction is **well formed** if each READ, WRITE and UNLOCK operation is covered earlier by a corresponding lock operation.
- A history is **legal** if does not grant conflicting grants.
- A transaction is **two phase** if its all lock operations precede its unlock operations.
- **Locking theorem**: If all transactions are **well formed** and **two-phased**, then any **legal** history will be isolated.
- **Locking theorem (Converse)**: If a transaction is **not well formed** or is **not two-phase**, then it is possible to write another transaction such that it is a **wormhole**.
- **Rollback theorem**: An update transaction that does an UNLOCK and then does a ROLLBACK is not two phase.

#### Degrees of isolation

- Degree 3: A Three degree isolated Transaction has no lost updates, and has repeatable reads. This is “true” isolation. (read write两段锁, read write都有锁)
  - sensitive to: write->write; write ->read; read->write
- Degree 2: A Two degree isolated transaction has no lost updates and no dirty reads. (write锁是两段，read write都有锁)
  - sensitive to: write->write; write ->read
- Degree 1: A One degree isolation has no lost updates. (write两段锁)
  - sensitive to: write->write
- Degree 0 : A Zero degree transaction does not overwrite another transactions dirty data if the other transaction is at least One degree. (write加锁)
  - It ignores all conflicts

## 8th Week

## 9th Week

## 10th Week

## 11th Week
