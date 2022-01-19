## What is Garbage Collector

- Garbage collection is a **memory management technique.** It is a separate automatic memory management method which is used in programming languages where manual memory management is not preferred or done.
- In the **manual memory management** method, the user is required to mention the memory which is in use and which can be deallocated, whereas **the garbage collector** collects the memory which is occupied by variables or objects which are no more in use in the program.
- Only **memory will be managed by garbage collectors**, other resources such as destructors, user interaction window or files will not be handled by the garbage collector.

- Few languages need garbage collectors as part of the language for good efficiency. These languages are called as garbage-collected languages.
    - For example, `Java, C#` and most of the scripting languages needs garbage collection as part of their functioning.
    - Whereas languages such as `C and C++`support manual memory management which works similar to the garbage collector.
- There are few languages that support both garbage collection and manually managed memory allocation/deallocation and in such cases, a separate heap of memory will be allocated to the garbage collector and manual memory.
- Some of the bugs can be prevented when the garbage collection method is used. Such as:
    - dangling pointer problem in which the memory pointed is already deallocated whereas the pointer still remains and points to different reassigned data or already deleted memory
    - the problem which occurs when we try to delete or deallocate a memory second time which has already been deleted or reallocated to some other object
    - removes problems or bugs associated with data structures and does the memory and data handling efficiently
    - memory leaks or memory exhaustion problem can be avoided

- Let's see a detailed understanding of manual memory management vs garbage collection, advantages, disadvantages and how it is implemented in C++.

## Manual Memory Management

```c++
n = new sample_object;

delete n;
```

- Dynamically allocated memory during run time from the heap needs to be released once we stop using that memory. Dynamically allocated memory takes memory from the heap, which is a free store of memory.
- In `C++` this memory allocation and deallocation are done manually using commands like new, delete. Using 'new' memory is allocated from the heap. After its usage, this memory must be cleared using the 'delete' command.

- As shown, every new should end or incline with a delete command. Here n pointer is allocated memory using 'new' command and is referenced or pointed to an object called 'sample_object'. Once the usage and functioning of the pointer are completed, we should release or free the memory using the 'delete' command as done above.
- But in case of garbage collection, the memory is allocated using 'new' command but it need not be manually released using 'delete'. In such cases, the garbage collector runs periodically checking for free memory. When a piece of memory is not pointed by any object it clears or releases the memory creating more free heap space.

## Advantages and Disadvantages of Manual Memory Management

- Advantages of manual memory management are that the user would have complete control over both allocating and deallocating operations and also know when a new memory is allocated and when it is deallocated or released. But in the case of garbage collection, at the exact same instance after the usage the memory will not be released it will get released when it encounters it during the periodic operation.

- Also in the case of manual memory management, the destructor will be called at the same moment when we call the ‘delete’ command. But in case of garbage collector that is not implemented.

- There are a few issues associated with using manual memory management. Sometimes we might tend to double delete the memory occupied. When we delete the already deleted pointer or memory, there are chances that the pointer might be referencing some other data and can be in use.

- Another issue which we have in manual memory management is, if we get an exception during the execution or usage of the new memory allocated pointer, it will go out of the sequence of ‘new’ and ‘delete’ and the release operation will not be performed. Also, there can be issues of memory leak.

## Advantages and Disadvantages of Garbage Collector

- One major disadvantage of garbage collection is the time involved or CPU cycles involved to find the unused memory and deleting it, even if the user knows which pointer memory can be released and not in use. Another disadvantage is, we will not know the time when it is deleted nor when the destructor is called.

## Garbage Collection Algorithm

- There are many garbage collection algorithms such as reference counting, mark, and sweep, copying, etc. Let’s see one algorithm in detail for better understanding. For example, when we see the reference counting algorithm, each dynamic memory will have a reference count. When a reference is created the reference count will increase and whenever a reference is deleted the reference count is decremented. Once the reference count becomes zero it shows that the memory is unused and can be released.

- This algorithm can be implemented in C++ using a specific pointer type. A specific pointer type should be declared and this can be used for purposes such as keeping track of all the references created, keeping track of the reference count when reference is created and deleted. A C++ program can contain both manual memory management and garbage collection happening in the same program. According to the need, either the normal pointer or the specific garbage collector pointer can be used.

- Thus, to sum up, garbage collection is a method opposite to manual memory management. In a garbage collector, the memory is released automatically based on a periodic time basis or based on specific criteria which tells if it is no more in use. Both methods have their own advantages and disadvantages. This can be implemented and used according to the complexity of the function, depending on the language used and its scope.

## Go - Garbage Collection

> The Go garbage collector helps developers by automatically freeing the memory of their programs when it is not needed anymore.

- [runtime 패키지에 mgc.go에 작성되어 있는 Go 언어 GC에 대한 설명](https://go.dev/src/runtime/mgc.go)

```go
// The GC runs concurrently with mutator threads, is type accurate (aka precise), allows multiple
// GC thread to run in parallel. It is a concurrent mark and sweep that uses a write barrier. It is
// non-generational and non-compacting. Allocation is done using size segregated per P allocation
// areas to minimize fragmentation while eliminating locks in the common case.
```