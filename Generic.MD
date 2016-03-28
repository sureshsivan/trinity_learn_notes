## Processes vs Threads
(http://www.programmerinterview.com/index.php/operating-systems/thread-vs-process/)
A process is an executing instance of an application. What does that mean? Well, for example, when you double-click the Microsoft Word icon, you start a process that runs Word. A thread is a path of execution within a process. Also, a process can contain multiple threads. When you start Word, the operating system creates a process and begins executing the primary thread of that process.

It’s important to note that a thread can do anything a process can do. But since a process can consist of multiple threads, a thread could be considered a ‘lightweight’ process. Thus, the essential difference between a thread and a process is the work that each one is used to accomplish. Threads are used for small tasks, whereas processes are used for more ‘heavyweight’ tasks – basically the execution of applications.

Another difference between a thread and a process is that threads within the same process share the same address space, whereas different processes do not. This allows threads to read from and write to the same data structures and variables, and also facilitates communication between threads. Communication between processes – also known as IPC, or inter-process communication – is quite difficult and resource-intensive.

## Event Loop
(http://blog.carbonfive.com/2013/10/27/the-javascript-event-loop-explained/)
(http://altitudelabs.com/blog/what-is-the-javascript-event-loop/)
(https://thomashunter.name/blog/the-javascript-event-loop-presentation/)


## How Browsers work
(http://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)


## Stream and Buffere
* stream is data flows through channel 
    1. ex - data coming through internet socket (TCP)
    2. Data coming from local machine - while reading file
* buffer is a place where chunk of data is collected which comes through stream.


## Charset and encoding, binary data
* binary data is data stored as 1's and 0's
* charsets like UTF-8/ASCII represent each character or letter or symbol in a number format
* Encoding is converting chacter to its binary format (any type of encoding like UTF8/ascii can be used)
    1. for UTF8 every char is converted to a binary of size 8 bits.
*    


## HEap and stack memory
* stack memory is local to function(only for local primitives) - when function executed the code(instruction) is pushed to STACK(LIFO)
* every function execution gets its own stack
* all the objects whether it is local to function of some other place like STATIC block gets its memory allocated to HEAP.
* all memory in stack (memory allocated to local primitives for a method) gets deallocated once the function execution completes.

# Pipes

# Internet Protocols
* TCP -Transmission control protocol
* UDP - USer Datagram Protocol
* TCP is better but slow and heavy weight due to integrity check - order preservations open connection and acknowledgement(SSH, HTTP, SMTP)
* UDP is fast but not reliable (DHCP, VOIP, IPTV)
* HTTP, SMTP, DHCP are application level protocols which are built on tops of base protocols like TCP/UDP