# Assignment 3 - Complete Documentation

**Student Name**: [LAYAN NASSER SHAYAHN]  
**Student ID**: [445052175]  
**Date Submitted**: [3 MAY]



## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: [`[445052175]_Assignment3_Synchronization.mp4`] https://drive.google.com/file/d/11Zg4Cbe5whr0m_Ht5kctr7x2qPxZjlrf/view?usp=drive_link

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [3-5-2026, 8:30]
**What I implemented**: 
 I reviewed the original CPU scheduler simulation and identified the shared resources that need synchronization, such as counters, total waiting time, and the execution log.
**Challenges encountered**: 
The main challenge was understanding which variables could be accessed by multiple threads at the same time
**How I solved it**: 
I marked the shared variables and planned to protect them using synchronization mechanisms.
**Testing approach**: 
I ran the program and checked the output to understand the current behavior.
**Time spent**: 
30 minutes.
---

### Entry 2 - [3-5-2026, 9:00]
**What I implemented**: 
I added a ReentrantLock to protect shared counters and the execution log
**Challenges encountered**: 
The challenge was making sure the lock is always released after each critical section
**How I solved it**: 
I used try-finally blocks so the lock is released even if an error occurs.
**Testing approach**: 
I ran the program and checked that the final statistics were displayed correctly.
**Time spent**: 
30 minutes.
---

### Entry 3 - [3-5-2026, 9:30]
**What I implemented**: 
I added a Semaphore with one permit to control CPU access.
**Challenges encountered**: 
The challenge was preventing multiple processes from using the CPU at the same time
**How I solved it**: 
I used cpuSemaphore.acquire() before execution and cpuSemaphore.release() inside a finally block.
**Testing approach**: 
I checked the output and confirmed that processes executed one at a time.
**Time spent**: 
30 minutes.
---

### Entry 4 -  [3-5-2026, 10:00]
**What I implemented**: 
I tested the program multiple times and verified the output statistics
**Challenges encountered**: 
needed to make sure the synchronization did not cause deadlock or incorrect results.
**How I solved it**: 
reviewed the lock and semaphore usage and ensured all resources were released properly.
**Testing approach**: 
compared the final completed process count, context switches, and log entries.
**Time spent**: 
45 minutes.
---

### Entry 5 - [3-5-2026, 10:45]
**What I implemented**: 
I completed the documentation and explained the synchronization techniques used
**Challenges encountered**: 
The challenge was explaining the technical concepts clearly
**How I solved it**: 
I connected each answer to my implementation and output results
**Testing approach**: 
I reviewed the code, output, and documentation together
**Time spent**: 
30 minutes.
---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

[Your Answer:
Two race conditions in the original code involve the shared counters and the execution log. The first race condition affects contextSwitchCount, completedProcessCount, and totalWaitingTime, because multiple threads may update these values at the same time. Concurrent access is a problem because operations such as contextSwitchCount++ are not atomic, so one update may overwrite another update. This could cause incorrect final statistics, such as the wrong number of completed processes or context switches. The second race condition affects executionLog, because ArrayList is not thread-safe. If multiple threads write to it at the same time, the log may become inconsistent or may cause a ConcurrentModificationException.]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[
ReentrantLock is used for mutual exclusion, meaning only one thread can enter a critical section at a time. I used it to protect shared data such as contextSwitchCount, completedProcessCount, totalWaitingTime, and executionLog. A Semaphore is used to control how many threads can access a limited resource at the same time. In my code, I used Semaphore(1) to represent a single CPU, so only one process can execute at a time. The lock protects data consistency, while the semaphore controls access to the CPU resource.]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

[
Deadlock is a situation where two or more threads are waiting for each other forever, so none of them can continue execution. One prevention technique is using try-finally blocks to make sure locks and semaphores are always released. Another technique is avoiding holding multiple locks at the same time, which reduces the chance of circular waiting. In my code, I used finally when unlocking the ReentrantLock and when releasing the CPU semaphore. This prevents a thread from keeping a resource forever if an exception happens]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[
In my implementation, I used one shared lock for the three counters and the execution log. This is a coarse-grained locking approach. I chose this because it is simpler, easier to understand, and reduces the chance of synchronization mistakes. The trade-off is that it may reduce concurrency because only one thread can update any protected shared resource at a time. Fine-grained locking would use separate locks for each counter, which can provide better concurrency because the counters are independent. However, it is more complex and requires careful design. Since the counters are independent, fine-grained locking would provide better concurrency in theory. But for this assignment, one lock is acceptable because the program is easier to maintain and still produces correct results]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextSwitchCount, completedProcessCount, and totalWaitingTime
**Why they need protection**: 
They are shared variables updated by multiple threads. Without protection, race conditions could cause incorrect final values
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
// Paste your implementation here
public static final java.util.concurrent.locks.ReentrantLock lock =
        new java.util.concurrent.locks.ReentrantLock();

public static void incrementContextSwitch() {
    lock.lock();
    try {
        contextSwitchCount++;
    } finally {
        lock.unlock();
    }
}

public static void incrementCompletedProcess() {
    lock.lock();
    try {
        completedProcessCount++;
    } finally {
        lock.unlock();
    }
}

public static void addWaitingTime(long time) {
    lock.lock();
    try {
        totalWaitingTime += time;
    } finally {
        lock.unlock();
    }
}
**Justification**: 
The lock ensures that only one thread can update the shared counters at a time, which prevents lost updates and keeps the final statistics correct.
---

### Critical Section #2: Execution Log

**What resource**: 
executionLog
**Why it needs protection**: 
It is an ArrayList, and ArrayList is not thread-safe. Multiple threads writing to it at the same time can cause inconsistent data
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
// Paste your implementation here
```public static List<String> executionLog = new ArrayList<>();

public static void logExecution(String message) {
    lock.lock();
    try {
        executionLog.add(message);
    } finally {
        lock.unlock();
    }
}

**Justification**: 
The lock protects the log from concurrent modification and ensures that log entries are added safely.
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
The semaphore controls access to the CPU simulation
**Number of permits and why**: 
I used 1 permit because the simulation represents one CPU, so only one process should execute at a time
**Where implemented**: 
The semaphore is declared in SharedResources and used inside run() and runToCompletion().
**Code snippet**:
```java
// Paste your implementation here
```public static final java.util.concurrent.Semaphore cpuSemaphore =
        new java.util.concurrent.Semaphore(1);

try {
    SharedResources.cpuSemaphore.acquire();

    // process execution code

} catch (InterruptedException e) {
    e.printStackTrace();
} finally {
    SharedResources.cpuSemaphore.release();
}

**Effect on program behavior**: 

It ensures that only one process executes at a time. This makes the scheduler behavior controlled and prevents multiple processes from using the simulated CPU simultaneously
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 
(
The program completed all processes successfully. The output showed 11 processes, time quantum 4000ms, total completed processes 11, total context switches 23, total waiting time 456263ms, average waiting time 41478ms, and total log entries 46)

**Why synchronization is necessary**: 
(
Synchronization is necessary because shared resources can be accessed by multiple threads. Without synchronization, counter values may be lost or incorrect, and the execution log may be corrupted. The lock protects shared variables, and the semaphore controls CPU access.)

**Conclusion**: 
The synchronization worked correctly and the final output was consistent
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException
tested whether the program avoids ConcurrentModificationException
**Testing procedure**: 
I ran the program and observed the execution log updates while processes were executing
**Results**: 
No ConcurrentModificationException occurred. The final output showed Total log entries: 46
**What this proves**: 
This proves that the execution log was protected properly using the lock
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)
I tested whether all processes completed and whether the final values were correct
**Expected values**: 
All 11 processes should finish execution. The completed process count should equal the number of processes
**Actual values**: 
Total completed processes = 11.
Total context switches = 23.
Total waiting time = 456263ms.
Average waiting time = 41478ms.
Total log entries = 46.
**Analysis**: 
The actual completed process count matches the expected number of processes. This shows that the scheduler completed all processes successfully and the shared counters were updated correctly
---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]
Running the scheduler with the generated time quantum of 4000ms and 11 processes
**Purpose**: 
The purpose was to verify that the scheduler can handle multiple processes with different burst times and priorities
**Results**: 
Some processes finished in one quantum, such as P2 and P6, while other processes needed multiple turns, such as P4, P7, and P9
**What I learned**: 
I learned that processes with burst time greater than the time quantum need more than one execution cycle. I also learned that synchronization is important even when the scheduler appears to run sequentially, because shared variables are still accessed by threads
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[I learned that synchronization is important when multiple threads share the same data. Without synchronization, race conditions can happen and the program may produce incorrect results. I learned that ReentrantLock is useful for protecting critical sections such as counters and lists. I also learned that Semaphore is useful when a limited resource should only be accessed by a certain number of threads. In this assignment, the semaphore represented the CPU, so only one process could execute at a time. I learned that try-finally blocks are important because they guarantee that locks and semaphores are released. I also understood that synchronization improves correctness, but it must be designed carefully to avoid deadlocks and performance issues]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 
Banking systems. Synchronization is critical when multiple users access or update the same bank account at the same time
**Example 2**: 
Online ticket booking systems. Synchronization prevents two users from booking the same seat at the same time.
---

### How I would explain synchronization to others:

[Synchronization is like having one key for a shared room. If many people want to enter the room and change something inside, only one person should enter at a time. When that person finishes, they return the key so someone else can enter. In programming, the shared room is the shared data, and the key is the lock. This prevents people, or threads, from changing the same thing at the same time and causing mistakes.]

---

## Part 6: GitHub Repository Information

**Repository URL**: 
https://github.com/Layannasser1/OS-Assignment3-layan-shayhan
**Number of commits**: 
9
**Commit messages**: 
1. Set student ID
2. Add Semaphore to control concurrent CPU access
3. Add ReentrantLock to protect execution log
4. protect shered variable  totalWaitingTime

---

## Summary

**Total time spent on assignment**: 
3 hours.
**Key takeaways**: 
1. Shared resources must be protected to prevent race conditions.
2. ReentrantLock protects critical sections, while Semaphore controls access to limited resources
3. try-finally blocks are important to prevent deadlocks and resource leaks

**Most challenging aspect**: 
The most challenging aspect was deciding where synchronization was needed and making sure locks and semaphores were released correctly
**What I'm most proud of**: 
I am most proud that the program runs successfully, completes all 11 processes, and produces correct synchronization statistics
---

**End of Documentation**
