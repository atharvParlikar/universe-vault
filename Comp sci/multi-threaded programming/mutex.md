## TLDR
- - -
Mutex is talking pillow for threads,
use mutex for multithreaded programming if you have global shared resources.
- - -

mutex is a synchronization mechanism to prevent multiple threads from accessing and changing a global shared value.

### How does a mutex work / how to use it.

To prevent two threads from accessing the global shared resource at the same time (to prevent [[race conditions]]), mutex basically locks the resource while a single thread is using it,

so suppose t1 and t2 want to use resource R

```
// t1
mutex.lock()
use the resource
mutex.unlock()

// t2
mutex.lock()
use the resource
mutex.unlock()
```

so because of the lock if t1 is using it t2 would have to wait and vice versa.

mutex is just a global integer value,
```c
int mutex = 1; // 0 := unlocked | 1 := locked
```

### Why there is no race condition for mutex.

If mutex is just a global shared variable, technically it is also prone to [[race conditions]] as every other global shared resource.
But that does not happen because mutex is implemented with
[[atomic instructions]].

In short, Atomic instructions makes it physically impossible on a processor level to run any other instructions while they are running thus, when you lock or unlock a mutex that is the only instruction running on that processor.

here is how a mutex implemented with and without atomic instructions,

#### Without atomic instructions (Bad mutex)
```assembly
lock_acquire:
    lw $t0, 0($a0)
    bne $t0, 1, lock_acquire
    li $t1, 0
    sw $t1, 0($a0)
````

#### With atomic instructions (Good mutex)
```assembly
lock_acquire:
    li $t0, 0
    li $t1, 1
    cas $t0, $t1, lock    # Atomic instruction
    beq $t0, $t1, lock_acquire
```