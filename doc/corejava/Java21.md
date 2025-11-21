## What Are Virtual Threads?

A virtual thread in Java is a lightweight, JVM-managed thread introduced in Java 21 as part of Project Loom to massively simplify and scale concurrent applications.

It behaves like a normal Java thread but is not backed 1:1 by an OS thread.

~~~java
import java.util.*;

public class TestVT {
public static void main(String[] args) throws Exception {

        int n = 1000;
        List<Thread> threads = new ArrayList<>();

        for (int i = 1; i <= n; i++) {
            int id = i;
            Thread vt = Thread.startVirtualThread(() ->
                System.out.println("Virtual Thread " + id + " running")
            );
            threads.add(vt);
        }

        // Wait for all threads to finish
        for (Thread t : threads) {
            t.join();
        }

        System.out.println("All virtual threads completed.");
    }
}
~~~
### Virtual Threads vs ExecutorService (OS Threads)

| Feature           | Virtual Threads                    | ExecutorService (OS Threads) |
| ----------------- | ---------------------------------- | ----------------------------- |
| **Thread type**       | User-mode (JVM-managed)            | OS thread                     |
| **Max threads**       | Millions                           | Few hundred                   |
| **Good for**          | I/O tasks (DB, network, API calls) | CPU-heavy tasks               |
| **Memory per thread** | Few KB                             | ~1–2 MB                       |
| **Scheduling**        | JVM scheduler                      | OS scheduler                  |
| **Blocking calls**    | Cheap                              | Expensive                     |
| **Deadlocks**         | Less likely                        | More likely                   |
| **Creation cost**     | Very low                           | High                          |
| **Need pool?**        | **No**                             | Yes                           |
| **Best usage**        | "Thread per request" model         | Limited concurrency           |