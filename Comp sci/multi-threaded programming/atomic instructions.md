**Atomic instructions** are special low-level operations provided by the CPU that ensure a sequence of steps is executed as a single, indivisible unit.

This means that when an atomic instruction is running, no other thread or process can interrupt it. It’s like temporarily pausing the rest of the system to ensure the operation completes without interference.

### Why Are Atomic Instructions Important?

When accessing or modifying shared resources, [[race conditions]] can occur if multiple threads execute non-atomic operations simultaneously. Mutexes, for example, rely on atomic instructions to lock or unlock safely. Without these, mutexes themselves could fall prey to race conditions.

---

### Example: Atomic vs. Non-Atomic Operations

#### Non-Atomic Increment (Prone to Race Conditions)

```assembly
load $t0, count  # Load count into a register
add  $t0, $t0, 1 # Increment
store $t0, count # Store the incremented value back
```

Here, another thread can modify `count` between the `load` and `store`, causing inconsistencies.

#### Atomic Increment (Safe)

```assembly
atomic_increment count  # Performs load, add, and store atomically
```

In this case, the operation is indivisible, so no other thread can access `count` during the update.

Atomic instructions form the backbone of synchronization mechanisms like [[mutex]] and are critical for building thread-safe programs.
