# DAILY TEST 3: MULTI-THREADED PROGRAMMING, DATA FILES, AND NETWORK PROGRAMMING (JAVA ONLY)

## Multiple Choice Questions

### Question 1
In Java, which of the following is NOT a valid way to create a new thread?
- A) Extending the `Thread` class
- B) Implementing the `Runnable` interface
- C) Using the `ForkJoinPool` class directly
- D) Using an `ExecutorService`

**Answer: C) Using the `ForkJoinPool` class directly**

**Explanation:** While `ForkJoinPool` is used for parallel task execution, it's not a direct way to create a simple thread. Threads are typically created by extending `Thread` or implementing `Runnable` and then managed using `ExecutorService` or started directly. `ForkJoinPool` is a specialized `ExecutorService` for recursive fork/join tasks.

**Knowledge Area:** Multi-threaded Programming (Java)

### Question 2
What is the primary purpose of the `synchronized` keyword in Java multithreading?
- A) To increase the speed of thread execution
- B) To ensure that only one thread can access a critical section of code at a time
- C) To allow threads to run in parallel on different CPUs
- D) To automatically manage thread lifecycle

**Answer: B) To ensure that only one thread can access a critical section of code at a time**

**Explanation:** The `synchronized` keyword in Java is used to achieve thread safety by controlling access to shared resources. It ensures mutual exclusion, meaning only one thread can execute a synchronized block or method on a given object at any time, preventing race conditions.

**Knowledge Area:** Multi-threaded Programming (Java)

### Question 3
Which interface must be implemented to define a task that can be executed by a thread in Java?
- A) `Thread`
- B) `Task`
- C) `Runnable`
- D) `Executable`

**Answer: C) `Runnable`**

**Explanation:** The `Runnable` interface in Java is designed for tasks that are intended to be executed by a thread. Classes implementing `Runnable` must define a `run()` method containing the code to be executed by the thread.

**Knowledge Area:** Multi-threaded Programming (Java)

### Question 4
What is a "race condition" in Java multithreading?
- A) A state where threads compete for CPU time
- B) A situation where multiple threads try to access and modify shared data concurrently, leading to unpredictable outcomes
- C) When a thread finishes its execution faster than expected
- D) When threads are waiting for I/O operations to complete

**Answer: B) A situation where multiple threads try to access and modify shared data concurrently, leading to unpredictable outcomes**

**Explanation:** A race condition occurs when multiple threads access shared data concurrently, and the final result of the operation depends on the specific order in which the threads execute. This can lead to unexpected and incorrect results if synchronization mechanisms are not properly used.

**Knowledge Area:** Multi-threaded Programming (Java)

### Question 5
Which class in `java.util.concurrent` is used to create and manage a pool of threads?
- A) `ThreadPool`
- B) `ThreadManager`
- C) `ExecutorService`
- D) `ConcurrentPool`

**Answer: C) `ExecutorService`**

**Explanation:**  `ExecutorService` is the interface in `java.util.concurrent` that represents an asynchronous execution service capable of executing `Runnable` tasks.  Implementations like `ThreadPoolExecutor` (obtained via `Executors` factory methods) provide thread pools to manage and reuse threads efficiently.

**Knowledge Area:** Multi-threaded Programming (Java)

### Question 6
What is "context switching" in the context of multithreading?
- A) Changing the priority of a thread during execution
- B) Switching the execution of a thread from one CPU core to another
- C) The process of saving the state of a running thread and loading the state of another thread to allow CPU sharing
- D) When a thread voluntarily yields control to another thread

**Answer: C) The process of saving the state of a running thread and loading the state of another thread to allow CPU sharing**

**Explanation:** Context switching is the mechanism that allows an operating system (and JVM) to quickly switch between threads, giving the illusion of concurrency. It involves saving the state of the current thread so it can be restored later and loading the saved state of another thread to run it.

**Knowledge Area:** Multi-threaded Programming (Java)

### Question 7
Which Java class is most suitable for efficiently reading large text files line by line?
- A) `FileReader`
- B) `FileWriter`
- C) `BufferedReader`
- D) `FileInputStream`

**Answer: C) `BufferedReader`**

**Explanation:** `BufferedReader` is designed for efficient reading of character streams, especially text files. It buffers input, reducing the number of disk read operations, making it much faster than reading directly with `FileReader` for line-by-line processing of large text files.

**Knowledge Area:** File Handling (Java)

### Question 8
What is the correct mode to open a file for writing in Java, overwriting existing content if the file exists?
- A) "r" (read)
- B) "a" (append)
- C) "w" (write)
- D) "rw" (read and write)

**Answer: C) "w" (write)**

**Explanation:** In Java's file handling, the "w" mode (used with `FileWriter` or `FileOutputStream`) is for writing. It creates the file if it doesn't exist and overwrites the file if it does. "r" is for reading, "a" is for appending, and "rw" is for reading and writing without truncation.

**Knowledge Area:** File Handling (Java)

### Question 9
Which Java class is used for reading binary data from a file?
- A) `FileReader`
- B) `BufferedReader`
- C) `FileInputStream`
- D) `FileWriter`

**Answer: C) `FileInputStream`**

**Explanation:** `FileInputStream` is specifically designed for reading raw byte streams from files, making it the correct choice for binary data. `FileReader` and `BufferedReader` are for character streams (text), and `FileWriter` is for writing characters.

**Knowledge Area:** File Handling (Java)

### Question 10
Which class in `java.nio.file` provides static methods for common file operations like reading all lines, copying, and deleting files?
- A) `File`
- B) `Path`
- C) `Files`
- D) `FileSystem`

**Answer: C) `Files`**

**Explanation:** The `Files` class in `java.nio.file` is a utility class that provides a rich set of static methods for performing various operations on files and directories, including reading all lines, copying, moving, deleting, and creating files and directories.

**Knowledge Area:** File Handling (Java)

### Question 11
What is the purpose of `RandomAccessFile` in Java?
- A) To create temporary files
- B) To read and write files sequentially
- C) To read and write files at arbitrary positions
- D) To compress and decompress files

**Answer: C) To read and write files at arbitrary positions**

**Explanation:** `RandomAccessFile` allows for both reading and writing to a file, and importantly, it allows seeking to any position within the file. This is useful for scenarios requiring non-sequential file access, like database systems or file indexing.

**Knowledge Area:** File Handling (Java)

### Question 12
Which method of `java.nio.file.Files` is used to read all lines of text from a file into a `List<String>`?
- A) `readLines()`
- B) `getAllLines()`
- C) `readAllLines()`
- D) `linesToList()`

**Answer: C) `readAllLines()`**

**Explanation:** The correct method in `java.nio.file.Files` is `readAllLines(Path path)`. It reads all lines from a file as a `List<String>`, decoding bytes using the platform's default charset.

**Knowledge Area:** File Handling (Java)

### Question 13
In the OSI model, which layer does TCP operate, which is crucial for Java Socket programming?
- A) Physical Layer
- B) Network Layer
- C) Transport Layer
- D) Application Layer

**Answer: C) Transport Layer**

**Explanation:** TCP (Transmission Control Protocol), fundamental to Java Socket programming for reliable, connection-oriented communication, operates at the Transport Layer in the OSI model. This layer ensures ordered and reliable delivery of data between applications.

**Knowledge Area:** Network Programming (Java)

### Question 14
Which Java class is used on the client side to establish a TCP connection to a server?
- A) `ServerSocket`
- B) `Socket`
- C) `DatagramSocket`
- D) `URL`

**Answer: B) `Socket`**

**Explanation:** The `Socket` class in Java is used by clients to initiate a TCP connection to a server.  It represents one endpoint of a two-way communication link between two programs running over the network. `ServerSocket` is for the server-side to listen for incoming connections.

**Knowledge Area:** Network Programming (Java)

### Question 15
Which Java class is used on the server side to listen for incoming TCP connection requests?
- A) `Socket`
- B) `ServerSocket`
- C) `DatagramSocket`
- D) `InetAddress`

**Answer: B) `ServerSocket`**

**Explanation:** `ServerSocket` is used by server applications in Java to listen for incoming client connection requests over TCP. It waits for clients to attempt a connection on a specific port and then creates a `Socket` object to handle each connection.

**Knowledge Area:** Network Programming (Java)

### Question 16
What is a "port" in networking, relevant to Java socket programming?
- A) A type of network cable
- B) A physical connector on a network device
- C) A logical endpoint for communication on a host, identified by a number
- D) A protocol used for data encryption

**Answer: C) A logical endpoint for communication on a host, identified by a number**

**Explanation:** In networking, a port is a number that identifies a specific process or service running on a computer. It acts as a logical endpoint for network communication.  TCP and UDP ports are fundamental to socket programming for directing network traffic to the correct application.

**Knowledge Area:** Network Programming (Java)

### Question 17
Which Java class is used to create a UDP socket for sending and receiving datagrams?
- A) `Socket`
- B) `ServerSocket`
- C) `DatagramSocket`
- D) `MulticastSocket`

**Answer: C) `DatagramSocket`**

**Explanation:** `DatagramSocket` in Java is used for UDP (User Datagram Protocol) communication. Unlike TCP sockets (`Socket` and `ServerSocket`), `DatagramSocket` operates connectionlessly and is used to send and receive individual packets of data (datagrams).

**Knowledge Area:** Network Programming (Java)

### Question 18
Which Java class was traditionally used (before Java 11) to make HTTP requests?
- A) `HttpClient`
- B) `HttpURLConnection`
- C) `HTTPRequest`
- D) `WebClient`

**Answer: B) `HttpURLConnection`**

**Explanation:** Before the introduction of the modern `HttpClient` in Java 11, `HttpURLConnection` was the primary class in standard Java for making HTTP requests. It was used to open connections to HTTP servers, send requests, and handle responses.

**Knowledge Area:** Network Programming (Java)

### Question 19
Which Java package introduced a modern, non-blocking HTTP Client API in Java 11 and later?
- A) `java.net.url`
- B) `java.net.http`
- C) `java.net.netclient`
- D) `java.httpclient`

**Answer: B) `java.net.http`**

**Explanation:** The `java.net.http` package, introduced in Java 11, provides the new and improved `HttpClient` API. This API supports modern HTTP features, including HTTP/2, asynchronous requests, and reactive streams for handling request and response bodies.

**Knowledge Area:** Network Programming (Java)

### Question 20
What is the key difference between TCP and UDP protocols, relevant in Java networking?
- A) TCP is faster than UDP
- B) TCP is connectionless, UDP is connection-oriented
- C) TCP provides reliable, ordered delivery of data, while UDP is connectionless and unreliable
- D) UDP is used for text data, TCP for binary data

**Answer: C) TCP provides reliable, ordered delivery of data, while UDP is connectionless and unreliable**

**Explanation:** TCP (Transmission Control Protocol) is reliable and connection-oriented, guaranteeing ordered and error-checked delivery of data. UDP (User Datagram Protocol) is connectionless and unreliable, meaning it provides a simpler, faster transport mechanism without guarantees of delivery or order, suitable for applications where some data loss is acceptable and speed is paramount.

**Knowledge Area:** Network Programming (Java)

### Question 21
In Java multithreading, what is a "join" operation?
- A) Starting a new thread
- B) Terminating a thread forcefully
- C) Waiting for a thread to complete its execution
- D) Resuming a paused thread

**Answer: C) Waiting for a thread to complete its execution**

**Explanation:** The `join()` method in Java `Thread` class is used to make the current thread wait until the thread on which `join()` is called terminates. It is used to synchronize threads, ensuring that one thread waits for another to finish before proceeding.

**Knowledge Area:** Multi-threaded Programming (Java)

### Question 22
What is the purpose of the `volatile` keyword in Java when used with variables accessed by multiple threads?
- A) To make the variable immutable
- B) To ensure that the variable is always stored in CPU cache
- C) To guarantee that every thread sees the most up-to-date value of the variable, reading directly from main memory
- D) To prevent garbage collection of the variable

**Answer: C) To guarantee that every thread sees the most up-to-date value of the variable, reading directly from main memory**

**Explanation:**  The `volatile` keyword ensures that changes to a variable are immediately visible to all threads. It forces the variable to be read from and written to main memory, bypassing thread-local caches, thus guaranteeing visibility of updates across threads.

**Knowledge Area:** Multi-threaded Programming (Java)

### Question 23
Which Java class is used to serialize Java objects to binary streams for file storage or network transmission?
- A) `PrintWriter`
- B) `DataOutputStream`
- C) `ObjectOutputStream`
- D) `FileOutputStream`

**Answer: C) `ObjectOutputStream`**

**Explanation:** `ObjectOutputStream` in Java is used to perform object serialization, converting Java objects into a stream of bytes that can be written to a file or transmitted over a network.  This process is essential for saving object states and for remote method invocation (RMI).

**Knowledge Area:** File Handling (Java)

### Question 24
What is the advantage of using a Thread Pool in Java compared to creating new threads for each task?
- A) Thread pools increase CPU usage
- B) Thread pools reduce the overhead of thread creation and destruction, improving performance and resource management
- C) Thread pools guarantee deadlock prevention
- D) Thread pools simplify thread synchronization

**Answer: B) Thread pools reduce the overhead of thread creation and destruction, improving performance and resource management**

**Explanation:** Thread pools significantly reduce the overhead associated with creating and destroying threads for each task. By reusing threads, they improve performance, especially for applications with many short-lived tasks, and also help manage resource consumption by limiting the number of concurrently running threads.

**Knowledge Area:** Multi-threaded Programming (Java)

### Question 25
Which of the following is a common technique to prevent race conditions in Java multithreaded programming?
- A) Increasing thread priority
- B) Using more threads to handle tasks
- C) Using synchronization mechanisms like `synchronized` blocks or locks
- D) Using UDP instead of TCP

**Answer: C) Using synchronization mechanisms like `synchronized` blocks or locks**

**Explanation:** Race conditions are primarily prevented by using synchronization mechanisms. Java provides `synchronized` keywords, locks from `java.util.concurrent.locks`, and other concurrency utilities to control access to shared resources and ensure that critical sections of code are executed by only one thread at a time, thus avoiding data corruption and unpredictable behavior.

**Knowledge Area:** Multi-threaded Programming (Java)

### Question 26
Which method of a `ServerSocket` is used to accept an incoming client connection in Java?
- A) `connect()`
- B) `listen()`
- C) `accept()`
- D) `bind()`

**Answer: C) `accept()`**

**Explanation:** The `accept()` method of the `ServerSocket` class is a blocking method that waits for an incoming client connection. When a client attempts to connect to the server's port, `accept()` returns a new `Socket` object representing the connection to that client, allowing the server to communicate with the client.

**Knowledge Area:** Network Programming (Java)

### Question 27
In Java NIO (Non-blocking I/O), which component is used for multiplexing I/O operations, allowing a single thread to manage multiple channels?
- A) `Thread`
- B) `Selector`
- C) `Channel`
- D) `Buffer`

**Answer: B) `Selector`**

**Explanation:** In Java NIO, a `Selector` allows a single thread to monitor multiple `Channels` for events such as readiness for reading or writing. This multiplexing capability is key to non-blocking I/O, enabling efficient handling of many connections with fewer threads compared to traditional blocking I/O.

**Knowledge Area:** Network Programming (Java)

### Question 28
What is the purpose of `try-with-resources` statement in Java when working with files?
- A) To automatically compress files after use
- B) To automatically close resources (like files or sockets) after they are no longer needed, ensuring resource management and preventing leaks
- C) To handle exceptions that occur during file operations
- D) To optimize file reading and writing speed

**Answer: B) To automatically close resources (like files or sockets) after they are no longer needed, ensuring resource management and preventing leaks**

**Explanation:** The `try-with-resources` statement in Java automatically closes resources declared in its try block (like `BufferedReader`, `FileWriter`, `Socket`, etc.) once the block is exited, whether normally or due to an exception. This ensures proper resource management and avoids resource leaks, especially crucial for file and network operations.

**Knowledge Area:** File Handling (Java)

### Question 29
Which Java class is used for reading and writing primitive data types in a machine-independent way, especially useful in file I/O?
- A) `BufferedReader` and `PrintWriter`
- B) `FileReader` and `FileWriter`
- C) `DataInputStream` and `DataOutputStream`
- D) `FileInputStream` and `FileOutputStream`

**Answer: C) `DataInputStream` and `DataOutputStream`**

**Explanation:** `DataInputStream` and `DataOutputStream` in Java are used to read and write primitive data types (like `int`, `float`, `boolean`, etc.) in a binary format that is machine-independent. This is particularly useful for file I/O or network communication where data needs to be consistently interpreted across different systems.

**Knowledge Area:** File Handling (Java)

### Question 30
 In Java, what is the role of `Paths.get()` method in the `java.nio.file` package?
- A) To delete a file or directory
- B) To create a new file or directory
- C) To obtain a `Path` object by converting a path string or URI
- D) To check if a file or directory exists

**Answer: C) To obtain a `Path` object by converting a path string or URI**

**Explanation:** The `Paths.get()` method in Java NIO is used to create `Path` instances. `Path` is an interface representing a path in a file system. `Paths.get()` can take a string or URI and convert it into a `Path` object, which can then be used with other NIO classes like `Files` for file and directory operations.

**Knowledge Area:** File Handling (Java)