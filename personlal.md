## About me

### Personal Information

● Name: Ye Yuan
● Major: Network Engineering
● University: Xi'an University of Posts
● Degree: Undergraduate sophomore
● Email: origin020726@gmail.com
● Github: https://github.com/Origin-yy
● TimeZone: Shanghai, China (GMT+8)

###Schedule

Before the summer vacation of school (around July 15th), I can dedicate 4-6 hours of free study time every day, averaging more than 30 hours per week, in addition to school courses. Of course, there will be mid-term and final exams.

After the summer vacation, according to the group's habits, I will stay on campus to study and spend the whole summer studying in the group. We have a scheduled timetable, with 8+ hours of group study every day from Monday to Saturday, and the study content is arranged by ourselves. There are no requirements on Sundays, and there will be one outing activity.

During the entire GSoC project period, I have no other major activities planned, and most of this free time will be used to learn technology, with completing the GSoC project as the top priority.

### Educational Experience

In September 2021, I entered the Network Engineering major at Xi'an University of Posts and Telecommunications and began to study computer network-related knowledge. In November 2021, I became a member of XiYou Linux Group and started to learn about Linux. From 2022 until now, I have studied Linux system programming and network programming, including Linux system calls, multi-threading and multi-processing, thread pools, TCP/IP protocol, I/O multiplexing, socket communication, and transitioned from C to C++.

### Code Experience

+ Familiarity with common data structures and algorithm problems.
+ Implemented the "ls" command using C language and Linux system calls, supporting flexible combinations of parameters such as -a, -l, -s, -t, -r, -i, and -R.
+ Implemented a shell using C language, inter-process communication and Linux system calls, supporting operations such as pipes, redirects, background running, and changing directory.
+ Implemented producer-consumer and dining philosophers problems using C and C++ multi-threading, as well as a thread pool.
+ Developed a C++ multi-user network chat room on a LAN using socket, thread pool, I/O multiplexing, and redis. The clients and servers are highly stable and concurrent, supporting common operations such as group chat, adding/deleting friends, file sending, and offline messaging, with data persistence using redis.

## Project Description

### Description

ibverbs and libfabric are both libraries used for high-performance network communication, but rFaaS can only use the network protocols supported by ibverbs and cannot use other network protocols. Therefore, we need an implementation using libfabric that supports other types of networks. This project has two main goals:

1. Refactor the existing codebase into two compatible implementations using either ibverbs or libfabric.
2. Support more types of networks by adjusting the libfabric configuration for TCP or AWS EFA protocols.

### The Scope of Work:

1. Introduce the libfabric network communication interface so that the project can complete communication tasks using libfabric.
2. Preserve the original ibverbs functionality and make it compatible with the introduced libfabric interface, handling communication based on different network environments and other requirements.
3. Modify the libfabric interface and configuration, add new content, and enable it to use appropriate transport layers for communication when encountering other network protocols, such as TCP or AWS EFA.
4. Write test cases, including support tests and performance tests under different network protocols.

### Complexity

1. The ibverbs and libfabric APIs are different, requiring a lot of code refactoring to adapt to the new API.
2. The ibverbs and libfabric have different resource management methods, requiring proper handling of the reallocation and management of resources such as Queue Pair (QP), Completion Queue (CQ), and Memory Region (MR) to ensure data consistency.
3. The new implementation must be compatible with the original one, maintain high performance and reliability, and be capable of handling different network environments.

### Related Work

Open MPI, which uses the communication interface provided by libfabric to enable application programs to switch between different transport layers. When ibverbs are needed, libfabric uses ibverbs as the underlying transport protocol to perform data transmission, providing high-performance and low-latency communication.

### Project Proposal

1. Use the existing ibverbs implementation as a basis to create a new module, moving all ibverbs-related code to this module. Provide the interface to the outside and ensure that the original functionality works properly.
2. Similarly, create a new module, using libfabric to implement the same functionality as ibverbs. Enable the original functionality to work properly when using libfabric.
3. Define a unified communication layer interface for both modules, dealing with conflicts and differences between the two and providing a unified API for the upper layer. The upper layer application will interact with the communication layer interface instead of directly calling the ibverbs or libfabric interface. According to the current network environment, the new communication layer can select appropriate operations from the appropriate module to complete the upper layer calling task.
4. Write test cases to ensure that it works correctly under different network protocols, and perform performance testing in various scenarios.
5. In the libfabric module, add new parameters to indicate the user's selected network protocol. For different network protocols, handle their characteristics and limitations, modify the resource allocation, connection establishment, data transmission interfaces, etc., and achieve the goal of supporting TCP and AWS EFA networks through the user-specified network protocol.
6. Refine the libfabric module, adjust the original implementation, and handle different protocols accordingly to complete the same operations.
7. Write project documentation, providing detailed configuration and usage instructions.
8. Write test cases to ensure that it can work properly on TCP and AWS EFA networks and carry out performance testing.

### Timeline

|          Dates          | Work                                                         |
| :---------------------: | :----------------------------------------------------------- |
|      Before May 29      | Continue reading the existing ibverbs implementation, read the libfabric and ibverbs manuals, compare the similarities and differences in completing the same operations for upcoming module refactoring, and clarify the ideas. |
|  May 29th ~ June 19th   | Start coding, migrate existing code using ibverbs, create a new module, and ensure that original functionality works correctly. <br />Consider a modular structure similar to ibverbs for libfabric module implementation and outline the overall framework. <br />Test support and maintain original performance. |
|  June 19th - July 29th  | Create the libfabric module that can perform the same tasks as the ibverbs module.<br /> Abstract a new API interface that combines the implementation of both modules, selects appropriate operations from the appropriate module to complete the upper-layer calling task based on requirements. <br />Test support for the original functionality while adding support for new networks; debug and optimize to meet performance demands. |
| July 29th - August 20th | Add new parameters to the libfabric module to adapt to other network protocols. Debug and optimize the module to accommodate different protocols, ensuring network stability, robustness, and adaptability. <br />Test and measure performance improvements; ensure that it meets performance demands. |

#### Buffer Time (8 days)

There are 8 buffer days in case something didn’t go as planned in the weeks before.

## Others

Why did you choose this project? What made you interested in our organization?

rFaaS supports the high-performance, low-latency calling of network resources using RDMA. The relevant technology required for this project aligns with my learning direction, and I have a strong interest in it.

What do you wish to accomplish during GSoC?

I hope to learn new technologies that I'm interested in, improve my technical and communication collaboration abilities, and make contributions to open-source projects that I am passionate about.

Have you worked with the required technologies before? If yes, then in what scope? Which projects?

I have not used ibverbs, libfabric, or RDMA before. However, I have some knowledge of network programming, TCP/IP, C++, and thread pools through learning, which I gained in XiYou Linux Group, and can be found on my GitHub.

Did you contribute to open-source projects before? Have you ever made a pull request, helped to fix an issue, or developed a project?

No, this is my first time participating in an open source project.
