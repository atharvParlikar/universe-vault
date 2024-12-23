A **race condition** occurs when the behavior of a program depends on the timing or sequence of execution of multiple threads accessing shared resources.

This can lead to unpredictable outcomes because the threads "race" to access and modify the resource first.

### Example of a Race Condition

Suppose two threads, `t1` and `t2`, are updating the same global variable `count`:

```c
// t1
count += 1; 

// t2
count += 1;
```

Under normal conditions, we might expect `count` to increment twice. But without synchronization, the following can happen:

1. `t1` reads the value of `count` (e.g., `count = 5`).
2. Before `t1` writes the new value (`count = 6`), `t2` also reads `count` (still `count = 5`).
3. Both threads write `count = 6`, and the increment from one thread is lost.

To prevent this, synchronization mechanisms like [[mutex]] are used.