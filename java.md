## Java

Java is a multi-platform programming language. It means that it allows writing programs that can be executed at any platform.

The secret power is the Java Virtual Machine (JVM). There are JVMs designed for most of platforms (Linux, Windows, etc.) that compile the bytecodes (pre-compiled Java code) for the Operational System (OS).

Because of it's characteristics, we can say that with Java you can "write once and run anywhere".


1. Stream API
- Introduced in Java 8
- Used to proccess collections of objects
- A stream is a sequence of objects and not a data structure
- Takes input from Collections, Arrays or I/O Channels
- Supports various operations to generate a result
- The original data structure aren't changed
- Intermediate operations are allowed to generate new data structures
- Terminal operations mark the end of the stream and return the result

1.1 Intermediate operations
- map
- filter
- sorted

1.2 Terminal operations
- collect
- forEach
- reduce

<hr>

[Home](index.html)
