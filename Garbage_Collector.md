## Garbage Collector
### Memory management strategies
The heap memory, even if big enough for most programming tasks, is still limited, as it takes part of the physical memory (RAM) on the computer. So a program taking too much memory will eventually lead to crashes. Most programs have objects that at some point of the execution won't be used anymore, which means the memory may be freed and reused later.

Some programming languages (for example, C or C++) require programmers to allocate and free memory manually. Although this provides more control over the resources, manual memory management may be a challenging task, especially for beginners, resulting in errors and memory leaks.

The JVM utilizes automated memory management, which allows developers not to worry about memory while writing code and prevents possible programming mistakes. Memory is allocated in the JVM heap every time a program creates a new object and is freed using the garbage collection process.



### What is Garbage Collection?
The Garbage Collector (or GC) is a part of the JVM that frees up the memory in runtime for further use. There are many different garbage collection algorithms and implementations, but their work may be simplified into two common steps. The first step is to determine which parts of memory the program no longer uses (i.e., "garbage"), and the second step is to free these parts of memory. Additionally, a compacting operation may be performed after the deletion step — all currently used objects are moved next to each other to free a big contiguous memory region and avoid fragmentation.

In order to identify garbage, the JVM goes through all the objects and checks whether they are still reachable in the program. All objects that can't be reached from the program or from other reachable objects are considered "garbage", and the corresponding memory is freed up.

Some algorithms take into account additional information about objects, for example, the time since the object was created. Such algorithms are called generational garbage collection algorithms. It has been noticed that most objects in programs are used only for a short time after creation. Thus, the garbage collector doesn't need to examine every object in the heap on every run and focuses mainly on recently created objects (the "younger generation"), which reduces the garbage collection time.

### Running GC
Garbage collection is performed automatically while a program is running. The JVM handles all the dirty work itself, including the decision when to run the GC. It may happen, for example, in fixed time intervals or when there is no free heap memory left.

In most cases, developers don't need to think about how the garbage collector works and how to customize it. However, in modern high-load applications, this knowledge may be useful.

In your programs, you may use the following ways to request the GC to perform the job:

* calling System.gc();
* calling Runtime.getRuntime().gc().
Programmers aren't supposed to run the garbage collector manually, and these calls don't even guarantee the GC invocation. We strongly recommend using them only in a test environment.

### Example
Let's say that we have a code to check the used memory size (you can do it using Runtime.getRuntime().totalMemory() and Runtime.getRuntime().freeMemory()), and we put it in a program that works the following way:

* Print the used memory information before performing any operations.
* Create a bunch of new objects in a cycle for further use.
* Print the used memory information after the objects' creation.
* Perform the necessary operations (without creating new objects) so that the objects are no longer used in the code.
* 
The value from step 3 will be greater than the value from step 1, since each new object takes some part of the available memory. After step 4, the objects become unreachable from the code and the memory may be freed up by calling System.gc() or Runtime.getRuntime().gc(). Printing the used memory information after the garbage collector invocation will show a value less than the value from step 3 if garbage collection was really performed.

We will start by discussing a few ways of finding unnecessary objects and learn what object generation means and how this mechanism classifies objects. We will also tell you about the pitfalls of automatic garbage collection and about the tools developers can use to monitor the execution of a program.


### How to find unused objects
To remove garbage from memory, the GC needs to know which objects are dead. The idea is quite simple and clear, but in fact, it is a rather complex process based on various algorithms. In this section, we will tell you about two approaches to locating unused objects:

* Reference counting. The idea of this approach is that each object is assigned a field showing the number of references pointing to it. Adding a reference increases its value by 1, and removing a reference decreases its value by 1.
Reference counting allows the collector to delete an object as soon as the counter value is zero, although this may not always be the right decision. This feature is one of the advantages of reference counting, but it also has some disadvantages. The application consumes memory to store the field for the reference counter, and its performance is reduced due to operations to increase or decrease the field value. Another important disadvantage is difficulties in finding circular references: when two objects refer only to each other, they may be invisible to the collector because their reference counter will never become 0.

* Tracing. This approach is more common than reference counting; in fact, JVM uses only tracing algorithms for garbage collectors. The tracing algorithm finds referenced objects, marks them, and writes off the rest as garbage. The search starts with objects known as GC Roots and builds a chain of related objects. Typical GC Root objects are local variables and method parameters, threads, static variables. In the image below, the blue circles represent chains of live objects, and the white ones represent dead objects, where the connection between a and b is an example of a circular reference.

![img_13.png](img_13.png)

An important advantage over the previous method is the possibility to find circular references but in this case, the garbage collector needs to wait until the algorithm finds all livе objects before it starts removing dead ones.
Simply put, reference counting and tracing are two opposites: the first tracks dead objects and the second tracks live ones. All algorithms performing garbage collection use these two approaches. Some prefer one of them, and some others use both.

### Memory cleanup: generational hypothesis
The generational mechanism is a common strategy for garbage collectors. It works by dividing objects into groups (generations), and if cleaning one group frees up enough memory, the collector does not clean the others, saving time and resources. As for the JVM, it has both generational and non-generational garbage collectors. For instance, G1 (Garbage First) uses a generational approach, and ZGC does not.

This approach divides objects into generations, depending on how many garbage collections they have survived. Memory cleanup starts with the youngest generation, where all new objects are. The reason for that is that the experience and statistics collected over the years have formed the generational hypothesis, which states that most objects die young. Therefore, GC must start garbage collection from the place where the youngest objects are stored. Objects surviving the garbage collection move to the next generation. In the second generation, the garbage collection is performed only when cleaning the first one does not free up enough memory. That is, the cleaning of each generation is carried out when the cleaning of all previous ones does not free up enough memory, and the objects that survived the cleaning move to the next generation. This way, passing through all generations, objects can reach the last one and remain there if the garbage collector does not delete them.

In addition to dividing objects into generations, there is another approach to organizing the memory management process – the division of memory into regions, where old and new objects can be stored in the same region. Java has GCs that use only this approach. It is also possible to use both of them in the same collector.
### Automated garbage collection pitfalls
While automatic garbage collection makes programming easier and speeds up application development, it also has its downsides. Among them are:

* **Resource consumption (memory, CPU)**. Automatic garbage collection consumes a lot of CPU and memory resources. This is the main reason why programming languages with manual memory management such as C++ work faster. In C++ developers control memory management themselves, and the application does not need to spend resources on it.
* **Latency**. All implementations of JVM garbage collectors have the so-called stop-the-world mechanism requiring applications to pause. It is a period when new objects are not created and garbage collection is run. Тhe longer this pause, the slower the application will run.
* **Memory fragmentation**. After dead objects removal, the memory areas where they were located remain unused. Therefore, after garbage collection, objects are overwritten next to each other to remove the extra space. Of course, this process also affects the performance of the application.
* 
Java developers have a set of tools to monitor the execution and performance of the program 
and get statistics. Some examples of such tools are **jstat** or **Java Mission Control**, 
shipped with Java, or third-party applications like **VisualVM** and **JProfiler**. Study these and other tools carefully. They will give you valuable benefits.