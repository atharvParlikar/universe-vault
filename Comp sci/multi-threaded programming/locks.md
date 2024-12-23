
Lock is any mechanism that restricts access to shared resources.
locks encompasses different synchronization techniques including [mutexes](mutex)


## Spin Lock
- - - 
spin lock is a type of lock that checks constantly if the lock has been unlocked.

Think of it like this,
```
Thread: is it unlocked?
spinlock: No 
Thread: is it unlocked?
spinlock: No
Thread: is it unlocked?
spinlock: No 
Thread: is it unlocked?
spinlock: No
Thread: is it unlocked?
spinlock: No 
Thread: is it unlocked?
spinlock: No
Thread: is it unlocked?
spinlock: Yes
Thread: OK
```

As you can see this keeps the CPU thread busy and waste computational resources if the thread is taking decent amount of time.
Spinlock only makes sense if the threads using the lock are extremely fast in execution so locking and unlocking in only a few CPU cycles.
If that is not the case then spinlock is just wasting resources

