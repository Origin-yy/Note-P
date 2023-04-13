# Libfabric for Fast Function Invocations

# Project Description

## Description

Currently, rFaaS directly uses the API of IBverbs to implement high-performance and low-latency remote memory access through RDMA operations. However, the network types supported by the API of IBverbs are limited. To support other types of network protocols, we need to use a more advanced communication library, Libfabric, to perform RDMA operations, while also being compatible with the existing IBverbs implementation.

 There are two main goals for this project:

1. Refactor the existing codebase to use a compatible implementation of IBverbs and Libfabric.
2. Support more types of networks by adjusting Libfabric configurations for protocols such as TCP and AWS EFA.

This link has a more detailed description [click](https://github.com/spcl/.github/blob/main/profile/gsoc.md#rfaas-libfabric-for-fast-function-invocations).

## The Scope of Work

+ Proficient in C/C++.
+ Familiar with RDMA operations, such as registering/deregistering memory regions, creating/destroying QPs, initiating/receiving data transfer requests, error handling, etc.
+ Familiar with the working principles and application scenarios of different network protocols, such as InfiniBand, RoCE, TCP/IP, and AWS EFA.
+ Proficient in using the Libfabric library and IBverbs for RDMA operations.
+ Familiar with configuring and using the sockets provider and EFA provider libraries.
+ Familiar with the similarities and differences between Libfabric and IBverbs, and able to complete RDMA operations using both libraries' interfaces.
+ Proficient in writing and using tests, optimizing performance, and ensuring the reliability and performance of the code.

## Complexity

+ The Libfabric API is complex and difficult to learn.
+ Different network protocols have their own characteristics and a wide range of applications.
+ The interface of the API for IBverbs and Libfabric is significantly different, and compatibility issues between the two must be carefully addressed.
+ The specific operations required for Libfabric when using different network protocols are not exactly the same, and these differences need to be handled properly.
+ To optimize performance, the best solution must be selected according to the various application scenarios to ensure performance.
+ Ensuring reliability requires careful consideration of error handling, as the modified code supports multiple different network protocols and RDMA adapters.

## Related Work

[MVAPICH2](https://mvapich.cse.ohio-state.edu/) is a high-performance computing communication library based on Message Passing Interface (MPI). It has a universal abstraction layer called CCH3 that can interact with different underlying network drivers. It uses verbs provider and IBverbs on InfiniBand and RoCE networks and uses ofi provider and Libfabric on other networks.

## Project Proposal

Similar to MVAPICH2, we can modify the existing IBverbs interface to an abstract network communication API interface, so that the program can directly call this API interface for network communication.

In the new abstraction layer, we implement two implementations of IBverbs and Libfabric. We can modify the calling interface based on the existing structures and IBverbs implementation and add Libfabric implementation to the interface. When using InfiniBand and RoCE networks, we use the existing IBverbs implementation, and when using other networks, we use the newly added Libfabric implementation.

For adding Libfabric, we can use abstract base classes and subclasses, template classes and specialization, conditional compilation, and macros. Each method has its advantages and disadvantages, and we can choose the appropriate implementation according to our needs. Currently, we only need to implement IBverbs and Libfabric, and the support for new network types can also use the Libfabric provider. We have high performance requirements, even if this may lead to redundant and bloated code, increased compile time, and memory consumption. Based on these reasons, I think the method of template classes and specialization is more suitable. Other methods can be considered later based on specific requirements, but the current plan mainly consists of using template classes and specialization.

The specific process is as follows:

1. First, write the Libfabric implementation with GNI as the Libfabric provider to ensure support for Cray GNI network.

2. For most existing API interfaces, such as the Connection class, we need to make the following modifications:

   + Modify the existing class to a template class and modify the parameters to a T-type parameter.

   + When using InfiniBand and RoCE networks, specialize the template class for the parameters of the existing IBverbs interface, convert the T-type parameter to multiple required parameters, and migrate the existing IBverbs implementation to complete the use of IBverbs implementation.

   + When using other network types, specialize the template class for the parameters of the Libfabric interface to be added, convert the T-type parameter to multiple required parameters, use the GNI provider, configure and initialize it for fabric, and use some of the network communication operations that have been implemented in the existing libfabric branch to complete the new Libfabric implementation with unique interfaces of Libfabric and GNI provider.

   + Use gtest to perform unit tests to verify the correctness and performance of the classes and optimize them.

3. Due to the differences in the APIs provided by IBverbs and Libfabric and the differences in resource manipulation methods and most of the existing implementations targeting IBverbs, I believe that we may need to separate a small part of the Libfabric code to better manage Libfabric. This can not only ensure the readability of the code but also facilitate future support for other network types. For example, the management of some abstract resources unique to Libfabric, such as endpoints, domains, and transmit contexts.

4. Add other code using the GNI provider of Libfabric to ensure complete function implementation, including:

   + Storage of required resources and variables.

   + Fabric configuration and initialization, such as fabric domain, domain, endpoint, etc.

   + Basic network communication functions: active connection initiation, passive connection acceptance, disconnection, memory registration, sending and receiving data and messages, read events, bulk receive, bulk publish sending, queue polling, etc.

   + Release of endpoint, queue, and other resources.

5. Modify the calling interface of the program, test and optimize it, and ensure complete function, reliability, and performance.

6. Modify the Libfabric implementation and adjust the Libfabric configuration to support more types of networks such as TCP or AWS EFA. The main modifications include:

   + Adding new headers and library dependencies.

   + Adding new fabric initialization and configuration processes.

   + Adding the use of TCP and EFA transmission models, using TCP provider and EFA provider as the underlying network transmission layer.

   + Adding the use of APIs and data structures specific to TCP and EFA, such as struct tcpx_ep, struct efa_ep.

   + Adding the use of memory resource management methods specific to TCP and EFA, such as tcpx_xfer_entry_alloc(), efa_mr_cache_regattr().

   + Adding new API calls, such as fi_sendmsg(), fi_recvmsg().

7. Test the code in the new network environment, optimize the code, and ensure reliability and performance.

The following are the main template class specializations that need to be performed:

| Original Code     | Existing Functionalities                                     | Libfabric Specialized Class                                  | Possible APIs                                                |
| ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Address class     | Get available fabric providers.<br />Represents address of RDMA connection including IP and port. | Remove `fi_getinfo()`, `fi_fabric()` and other initialization operations, integrate these functions into the new Fabric class. <br />Store resource usage and variable information, initialize required parameters. <br />Use APIs inside  [fi_av_set](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_av_set.3.html)  and [fi_av](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_av.3.html) to manage IP addresses and ports. | `fi_av_open()`, `fi_close()`,<br />`fi_av_bind()`, `fi_av_insert()`,<br /> `fi_av_set()`, `fi_close()`... |
| RDMAActive class  | Initialize related resources of Fabric.<br />Actively connect to another node.<br />Close connection. | Remove initialization of Fabric resources, integrate these functions into new Fabric class.<br />Store the necessary struct and variable information, initialize parameters.<br />Use APIs inside  [fi_cm](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_cm.3.html)  mainly to establish connections, return endpoints and connection information. | `fi_connect()`,`fi_close()`, <br />`fi_getpeer()`...         |
| RDMAPassive class | Initialize Fabric.<br />Listen and stop listening.<br />Passively receive connection requests from other nodes. | Remove `fi_domain()` operation, integrate these functions into new Fabric class. <br />Use APIs mainly inside [fi_cm](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_cm.3.html) , supplemented by APIs inside [fi_cntr](https://ofiwg.github.io/libfabric/v1.17.1/man/fi_cntr.3.html), [fi_eq](https://ofiwg.github.io/libfabric/v1.17.1/man/fi_eq.3.html), [fi_cq](https://ofiwg.github.io/libfabric/v1.17.1/man/fi_cq.3.html) to establish connections, return endpoints and connection information. | `fi_listen()`,`fi_accept()`, <br />`fi_pep_bind()`,`fi_eq_open(),` <br />`fi_shutdown()`,`fi_close()`... |
| Connection class  | Use Libfabric or IBverbs for RDMA communication, including: <br />Initialization, publish/receive messages, poll events, close connection, bulk receive, publish write and atomic operations. | Store resource usage and variable information, initialize required parameters.<br />Use APIs inside [fi_tagged](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_tagged.3.html) and [fi_msg](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_msg.3.html) to publish buffer to receive incoming messages and initiate sending messages.<br />Use APIs inside [fi_rma](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_rma.3.html) to perform remote memory read/write operations.<br />Use APIs inside [fi_poll](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_poll.3.html) mainly to poll events. Use APIs inside [fi_atomic](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_atomic.3.html) to perform atomic operations on remote memory.<br />Use APIs inside [fi_cq](https://ofiwg.github.io/libfabric/v1.17.1/man/fi_cq.3.html) [fi_cntr](https://ofiwg.github.io/libfabric/v1.17.1/man/fi_cntr.3.html) to assist in the above operations. | `fi_trecv()`, `fi_tsend()`, <br />`fi_recv()`, `fi_recvv(),` <br />`fi_send()`, `fi_sendv()` ,<br />`fi_read()` `fi_write()`, <br />`fi_readv()` `fi_writev()`, `fi_poll_open()` `fi_poll()`, <br />`fi_wait()` `fi_atomicv()` <br />`post_atomic_fadd()`... |
| Buffer class      | Manage registration and deregistration of local and remote memory. | Store resource usage and variable information, initialize required parameters. <br />Use APIs inside [fi_mr](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_mr.3.html) to manage local and remote memory. | `fi_mr_reg()`, `fi_close()`, <br />`fi_mr_key()`, `fi_mr_raw_attr()` ，<br />`fi_mr_refresh()`... |

My current idea is to separate the operations of obtaining available provider information, fabric initialization, resource initialization, and some resource control as a new Fablic class. This includes the creation, initialization, and control of the fablic domain, domain, endpoint, counter, completion queue and event queue. Currently, these operations are scattered across the Address class, RDMAActive class, RDMAPassive class, and Connection class, which I believe is not conducive to unified management. However, specific operations still need to be carefully considered.

Here are the possible code additions:

| New Code     | Functionality                                                | Main Implementation                                          | Possible APIs                                                |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Fabric class | Store information about supported network protocols, available providers, selected adapters, fabric objects, domain objects, endpoint objects, counters, event queues, completion queues, etc.<br />Get structure `fi_info` and the information saved in it.<br />Create/close/manage fabric objects, domain objects, endpoint objects, and other related operations.<br />Operate counters, event queues, and completion queues. | Use APIs inside [fi_getinfo](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_getinfo.3.html) to obtain information about available providers.<br />Use APIs inside [fi_fabric](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_fabric.3.html), [fi_domain](https://ofiwg.github.io/libfabric/v1.17.1/man/fi_domain.3.html) , and [fi_endpoint](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_endpoint.3.html) to create and close fabric objects, domain objects, and endpoint objects.<br />Use APIs inside [fi_cntr](https://ofiwg.github.io/libfabric/v1.17.1/man/fi_cntr.3.html), [fi_cq](https://ofiwg.github.io/libfabric/v1.17.1/man/fi_cq.3.html) , and [fi_cq](https://ofiwg.github.io/libfabric/v1.17.1/man/fi_cq.3.html) to create and operate counters, event queues, and completion queues. | `fi_getinfo()`,`fi_fabric()`,<br /> `fi_close()`,`fi_domain()`， <br />`fi_endpoint()`,`fi_ep_bind()` ,<br />`fi_cntr_open()`,`fi_enable`() ,<br />`fi_cq_open()`,`fi_eq_open()`... |

In addition to the above-mentioned codes, there are other codes that need to be modified, such as the server, client, executor remote function call executor, fast_executor function computing service program, etc. In these codes, when using libfabric, the API and variables they use need to be replaced with Libfabric API and Libfabric variables. It should be noted that for different providers, their control capabilities for resources are not the same, and the Libfabric API used is also different.

## Timeline

| Week |           Dates           | Work                                                         |
| ---- | :-----------------------: | :----------------------------------------------------------- |
| 0    |       Before May 29       | Familiarize yourself with the project and continue submitting Pull Requests (PRs) and making contributions. Read [Libfabric man pages](https://ofiwg.github.io/libfabric/v1.17.1/man/) to master the use of the Libfabric API, learn about IBverbs and refine the project framework and specific code implementation. Learn about the implementation principles and related knowledge of Cray GNI network, TCP, AWS EFI network and corresponding provider API of Libfabric. Prepare for coding. |
| 1    |    May 29th ~ June 4th    | Add Fabric class.<br />Test: pass gtest unit tests.          |
| 2    |   June 5th ~ June 11th    | Refactor RDMAActive class.<br />Test: pass gtest unit tests. |
| 3    |   June 12th ~ June 18th   | Refactor RDMAPassive class.<br />Test: pass gtest unit tests. |
| 4    |   June 19th ~ June 25th   | Refactor Connection class, complete message publishing/receiving and batch receiving/publishing write functions.<br />Test: pass gtest. |
| 5    |   June 26th ~ July 2nd    | Continue refactoring the Connection class, complete polling events, atomic operations and other functions.<br />Test: pass gtest. |
| 6    |   July 3rd ～ July 9th    | Refactor Buffer class.<br />Test: pass gtest unit tests.     |
| 7    |   July 10th ~ July 16th   | Modify the API calls of the server, client, executor remote function executor, and fast_executor function computing service program, improve the overall functionality of the program, and handle possible errors.<br />Test: the program can run normally and complete network communication. |
| 8    |   July 17th ~ July 23th   | Modify warm_benchmark, parallel_invocations and other test programs, test and optimize network communication under the Cray GNI network.<br />Test: pass all tests and ensure performance and reliability. |
| 9    |   July 24th ~ July 30th   | Add resource initialization and environment configuration for TCP provider and EFI provider, and add new code to Fabric class, RDMAActive class, and RDMAPassive class.<br />Test: pass gtest. |
| 10   |  July 31th ~ August 6th   | Add new code to Connection class and Buffer class.<br />Test: pass gtest. |
| 11   | August 7th ~ August 13th  | Modify other parts of the program, improve the overall functionality of the program, and handle possible errors.<br />Test: the program can run normally under TCP and AWS EFI networks. |
| 12   | August 14th ~ August 20th | Modify warm_benchmark, parallel_invocations and other test programs, test and optimize communication under TCP and AWS EFI networks.<br />Test: pass all tests and ensure performance and reliability. |

### Buffer Time (8 days)

Allow for a buffer period of 8 days as a safety net, in case anything does not go as planned.

# About me

## Personal Information

● Name: Ye Yuan
● Major: Network Engineering
● University: Xi'an University of Posts
● Degree: Undergraduate sophomore
● Email: origin020726@gmail.com
● Github: https://github.com/Origin-yy
● TimeZone: Shanghai, China (GMT+8)

## Schedule

Before the summer vacation of school (around July 15th), I can dedicate 4-6 hours of free study time every day, averaging more than 30 hours per week, in addition to school courses. Of course, there will be mid-term and final exams.

After the summer vacation, according to the group's habits, I will stay on campus to study and spend the whole summer studying in the group. We have a scheduled timetable, with 8+ hours of group study every day from Monday to Saturday, and the study content is arranged by ourselves. There are no requirements on Sundays, and there will be one outing activity.

During the entire GSoC project period, I have no other major activities planned, and most of this free time will be used to learn technology, with completing the GSoC project as the top priority.

## Educational Experience

In September 2021, I entered the Network Engineering major at Xi'an University of Posts and Telecommunications and began to study computer network-related knowledge. In November 2021, I became a member of XiYou Linux Group and started to learn about Linux. From 2022 until now, I have studied Linux system programming and network programming, including Linux system calls, multi-threading and multi-processing, thread pools, TCP/IP protocol, I/O multiplexing, socket communication, and transitioned from C to C++.

## Code Experience

+ Familiarity with common data structures and algorithm problems.[code link](https://github.com/Origin-yy/Code_c/tree/main/Group_Task_C/My_ls)
+ Independently implemented the "ls" command using C language and Linux system calls, supporting flexible combinations of parameters such as -a, -l, -s, -t, -r, -i, and -R.[code link](https://github.com/Origin-yy/Code_c/tree/main/Group_Task_C/My_ls)
+ Independently implemented a shell using C language, inter-process communication and Linux system calls, supporting operations such as pipes, redirects, background running, and changing directory.[code link](https://github.com/Origin-yy/Code_c/tree/main/Group_Task_C/My_Shell)
+ implemented producer-consumer and dining philosophers problems using C multi-threading, as well as a thread pool.[code link](https://github.com/Origin-yy/Code_c/tree/main/Group_Task_C/Thread)
+ Independently developed a C++ multi-user network chat room on a LAN using socket, C++thread pool, I/O multiplexing, and redis. The clients and servers are highly stable and concurrent, supporting common operations such as group chat, adding/deleting friends, file sending, and offline messaging, with data persistence using redis.[code link](https://github.com/Origin-yy/Code_cc/tree/main/Chatroom)

# Others

+ **Why did you choose this project? What made you interested in our organization?**

  rFaaS supports the high-performance, low-latency calling of network resources using RDMA. The relevant technology required for this project aligns with my learning direction, and I have a strong interest in it.

+ **What do you wish to accomplish during GSoC?**

  I hope to learn new technologies that I'm interested in, improve my technical and communication collaboration abilities, and make contributions to open-source projects that I am passionate about.

+ **Have you worked with the required technologies before? If yes, then in what scope? Which projects?**

  I have not used IBverbs, Libfabric, or RDMA before. However, I have some knowledge of network programming, TCP/IP, C++, and thread pools through learning, which I gained in XiYou Linux Group, and can be found on my GitHub.

+ **Did you contribute to open-source projects before? Have you ever made a pull request, helped to fix an issue, or developed a project?**

  No, this is my first time participating in an open source project. I will do my best.