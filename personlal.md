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

Before the school's summer vacation (about July 15th), I can spend 4 to 6 hours of free study time every day, 30+ hours a week. Of course there will be 4 days of midterm and final exams.

After the summer vacation, I will stay in school to study as usual. I will study in a group throughout the summer vacation. We have a well-arranged schedule. From Monday to Saturday, I have 8+ hours of study in the group every day. There is no requirement on Sunday, and there will be An outing.

During the entire Gsoc project period, unless there are force majeure factors, the above time schedule is true. The spare time will be used to learn technology and ensure the completion of the Gsoc project as a priority. There are no other additional arrangements.

### Educational Experience

In September 2021, I entered the network engineering major of Xi'an University of Posts and Telecommunications. Become a member of XiYou Linux Group, and learn about Linux under the leadership of many outstanding seniors. Since 2022, I have learned Linux system programming and network programming, including Linux system calls, multi-threading and multi-process, thread pool, TCP/IP protocol, Socket communication, etc., and transitioned from C to C++.

### Code Experience

+ Use C language and Linux system calls to complete the realization of the command ls and the realization of the shell.
+ Thread pool programming in C and C++.
+ Use socket, thread pool, epoll, and redis to complete the multi-threaded and concurrent C++ multi-person network chat room under the LAN, support common operations, and use redis for data persistence.

## Project Description

### Description

1. Both ibverbs and libfabric are libraries for high-performance network communication. Currently, rFaaS can only use the network protocols supported by ibverbs, but cannot use other network protocols. Therefore, we need an implementation that uses libfabric and supports other types of networks. The project has two main goals:

   1. Refactor the existing codebase into two compatible implementations using ibverbs or libfabric.

   2. Support more kinds of networks by adjusting libfabric configuration for protocols like TCP or AWS EFA.

   The scope of work:

   1. Introduce the advanced network communication interface of libfabric, and replace the existing ibverbs interface with the libfaric interface.
   2. When using libfaric, if ibverbs operation is supported, use ibverbs for communication at the transport layer.
   3. Modify some interfaces and configurations of libfabric, and add new content so that it can use the appropriate transport layer for communication when encountering other network protocols such as TCP or AWS EFA.
   4. Write test cases, including support tests and performance tests when using different network protocols.

   Complexity:

   1. The APIs of ibverbs and libfabric are different, which involves a lot of code refactoring to adapt to the new API.
   2. ibverbs and libfabric have different resource management methods, and need to properly handle the reallocation and management of resources such as QP (Queue Pair), CQ (Completion Queue) and MR (Memory Region).
   3. The new implementation should be compatible with the original one, and try to maintain the original high performance and reliability to cope with many network environments.

### Related work

Open MPI，它使用 libfabric 提供的通信接口来通信，让应用程序在不同的传输层之间切换。当需要使用ibverbs时，libfabric 使用 ibverbs 作为底层传输协议来进行数据传输，从而提供高性能和低延迟的通信。

### Project Proposal

1. Based on the existing ibverbs implementation, create a new module and move all ibverbs-related codes into this module. Provide the interface to the outside, and ensure the normal operation of the original function.
2. Similar to the first step, create a new module in which libfabric is used to achieve the same functionality as ibverbs. When using libfabric, the original function also works normally.
3. Define a unified interface for the two modules, handle the conflicts and differences between the two, provide a unified API for the upper layer, and select the appropriate operation in the appropriate module to complete the upper layer call task according to the current network environment.
4. Write test cases to ensure that it can work normally under different network environments. And conduct performance tests in various scenarios.
5. In the libfabric module, add a new parameter to mark the network protocol selected by the user, and perform related operations through the protocol specified by the user.
6. Improve the libfabric module, adjust the original implementation, and perform corresponding processing for different protocols to complete the same operation.
7. Write project documentation and provide detailed configuration and usage instructions.
8. Write test cases to ensure that it can work properly on networks such as TCP and AWS EFA, and conduct performance tests.时间线

|          Dates          | Work                                                         |
| :---------------------: | :----------------------------------------------------------- |
|      Before May 29      | Continue to read the existing ibverbs implementation, continue to read the libfabric manual and ibverbs manual, compare the similarities and differences between the two to complete the same operation, and lay a good framework for the subsequent module reconstruction. |
|  May 29th ~ June 19th   | Migrate the existing code using ibverbs, complete the creation of new modules, and ensure the normal operation of the original functions. <br />Refer to the modularization of ibverbs to consider the idea of implementing libabric modules, and sort out the general framework. <br />Test: be able to complete the original task and maintain the original performance. |
|  June 19th - July 29th  | Creating a libfabric module can accomplish the same work as the ibverbs module, and abstract the same API interface. Combining the implementations of the two modules, you can select the appropriate operation in the appropriate module to complete the upper-layer call task according to the requirements. <br />Write test samples for testing and optimize the code. |
| July 29th - August 28th | In the libfabric module, add new parameters to adapt to other network protocols. <br />Write documentation and tests, and make final optimizations. |

#### Buffer Time (7 days)

There are 7 buffer days in case something didn’t go as planned in the weeks before.

## Others

Why did you choose this project? What made you interested in our organization?

I'm very interested in network communications and the cloud, and this project using RDMA connections to support high-performance and low-latency calls intrigued me. High performance and low latency, fast access and operation of network resources, obtaining services from the cloud, etc., let me see a new world.

What do you wish to accomplish during GSoC?

I was encouraged by my seniors to participate in Gsoc. This event is more like an opportunity for me to start participating in open source projects and contribute to projects I like. At the same time, you can learn technology and improve your ability to communicate and collaborate. It helps me understand the work process, see the wider world, and benefit my future work.

Have you worked with the required technologies before? If yes, then in what scope? Which projects?

The used technologies are introduced above, such as ibverbs , libfabric , RDMA have not been used.

Did you contribute to open-source projects before? Have you ever made a pull request, helped to fix an issue, or developed a project?

No, this is my first time participating in an open source project, and I only helped another Gsoc community to modify the documentation for compiling and running projects on ArchLinux.
