# MULTITHREADED WEB-SERVER

This project aims to implement a multithreaded webserver "demohttpdaemon". It is implemented in C/C++ and is compatible for UNIX-based platform.

## DESCRIPTION

As the name suggests, "demohttpdaemon" is a utility web-server with the purpose of binding to a given <ipaddress>:<port> and serving content from a given directory for corresponding incoming HTTP/1.0 requests. It returns the document found relative to the given directory.

### OPTIONS

Our web-server incorporates the following functionalities: -
- **-h :** Displays the various options available.
- **-d :** Enter the debugging mode. It stops the server from running as a daemon process in the background.
- **-l file :** All requests are logged to the given file.
- **-p port :** Listens to the port given by the user. If any specific port is not given, the server by default listens to port 8080.
- **-r dir :** Sets up the root directory for the http server.
- **-t time :** Used to set up the queueing time given by the user. If any duration is not specified, the default is taken to be 60secs.
- **-n numthread :** Used to set the number of execution threads. If any number is not specified, the default is taken to be 4 as most processes run in quad-core processors.
- **-s schedule :** Used to set the scheduling policy to First-Come-First-Serve (FCFS) or Shortest-Job-First (SJF). If not specified, FCFS is taken to be default.

## RUNNING THE PROJECT

- Clone the repository and Change the Directory
```bash
    git clone https://github.com/pinakibasu7/Demohttpdaemon.git
    cd Demohttpdaemon
```
- Compile the ```demohttpdaemon.cpp``` file.
```bash
    g++ demohttpdaemon.cpp -lpthread -o demohttpdaemon
```
- Run the output file with the above mentioned flags
```bash
    ./demohttpdaemon -d
```
- Create Clients and send requests to the ```demohttpdaemon```.

## WORKING

### CONCEPTS

- A **THREAD** is a basic unit of CPU utilization, which comprises of a thread-id, a program counter, a register set, and a stack. It shares the code section, data section, and other operating-system resources such as open files and signals with other threads. In other terms, we can say that thread is a path of execution within the process.

- A **CRITICAL SECTION** is a piece of code that must be run atomically. In other words, it is imperative that only one thread executes the section in a given moment because the code accesses shared resources. Multiple accesses to shared resources at a given instance may lead to data inconsistency.

- A **MUTEX** is a 'lock' that we set before accessing a shared resource and release after using it. This helps in making sure that no two threads have access to a critical section of code simultaneously. pthread_mutex_lock() and pthread_mutex_unlock() are the inbuilt library functions which are used to achieve set and unset of a shared resource.

- A **SEMAPHORE**, like a mutex lock, is one of the solutions to solving the critical section problem. It uses two atomic operations, wait and signal, for process synchronizations. The key difference here with mutex is that while mutexes are used to protect shared resources, semphores are mainly used for signalling. 

### PROTOCOL

'demohttpdaemon' mainly responds to GET and HEAD requests in accordance to RFC1945. It is basically an application levvel protocol with lightness and speed necessary for communicating via the Internet. GET returns the data of the requested file, while HEAD returns only the metadata of the requested file and not the actual content.

When a connection is made, demohttpdaemon responds with the appropriate HTTP/1.0 status code with the following headers: -
- **DATE :** The current timestamp (GMT).
- **SERVER :** A string which identifies the server and the version.
- **LAST-MODIFIED :** The timestamp in GMT of the filre's last modification date.
- **CONTENT-TYPE :** Image/Gif/Text/Html
- **CONTENT-LENGTH :** Size (Bytes) of the data returned.

### LOGGING

Logging for 'demohttpdaemon' can be enabled by using the -l flag, which logs every request in the format: -
"%a %t  %t %r %s %b"

- **%a :** Remote IP address.
- **%t :** Receiving time of request.
- **%t :** Assigning time of request by the scheduler.
- **%r :** First line of the request.
- **%s :** Request status.
- **%b :** Content length.

### FUNCTIONS

- **insertion() :** Inserts a new request into the queue.
- **extract_element() :** Retrieves the first element from the queue.
- **removeSJF() :** From all the elements present in the queue, retrieve the element having the shortest job. Here, file size information is considered and taken as job length for deciding on scheduling.
- **display() :** Display all requests present in the queue.
- **print_help() :** Displays various options that can be performed by the daemon.
- **thread_serve() :** Responsible for thread pooling. Implemented using Semaphores and Mutex Locks.
- **thread_scheduler() :** Thread responsible for extracting request as per FCFS/SJF. Implemented using Semaphores and Mutex Locks.
- **thread_listen() :** Thread responsible for listening to request and inserting into queue. Implemented using Semaphores and Mutex Locks.

**NOTE :** The code has been adequately commented for understanding the flow of events occuring within each function and can be referred for having an in-depth view.

## LEARNING

- Received a hands-on experience on the power of multi-threading and how multi-threading helps to reduce the wait time for a request.
- Understood practically how semaphores and mutex locks prevent race-condition in the system by allowing only thread to run at a time in a critical section.
- Learnt about scheduling in OS and implemented scheduling algorithms like SJF and FCFS.

## ADDITIONAL TASKS

- Implemented the scheduling algorithms First-Come-First-Serve (FCFS) and Shortest-Job-First (SJF) to schedule and synchronize the processing of threads, while at the same time preventing the system from going into race conditions and making sure shared variables are not accessed by multiple threads at once during execution.
- Further planning to implement Round-Robin algorithm and priority queueing based algorithms for better scheduling of threads with respect to their priority and needs (Not implemented yet, but will implement soon. Having to manage intern + project together).

## REFERENCES

- Operating System Concepts by Avi Silberschatz, Greg Gagne and Peter Galvin.
- Multithreaded Programming, IBM
- GeekforGeeks
- StackOverflow
- Thread Pools, Microsoft
- kturley.com
- ACM IITR Lectures of Multithreaded Servers.
