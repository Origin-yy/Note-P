## Project Description

### Description

Currently, rFaaS uses the IBverbs API for RDMA operations to achieve high performance and low-latency remote memory access. However, the IBverbs API is limited in the types of network protocols it supports. To support other types of network protocols, we need to use a higher-level communication library called Libfabric for RDMA operations while maintaining compatibility with the existing IBverbs implementation. This project has two main goals:

1. Introduce the Libfabric library and refactor the existing codebase to use a compatible implementation of IBverbs and Libfabric.
2. Adjust the Libfabric configuration for TCP or AWS EFA protocols to support a wider range of network types.

### The Scope of Work

- Familiarity with C/C++.
- Familiarity with RDMA operations, such as registering and deregistering memory regions, creating and destroying QPs, initiating and receiving data transfer requests, error handling, etc.
- Understanding of the working principles and application scenarios of different network protocols, such as InfiniBand, RoCE, TCP/IP, and AWS EFA.
- Understanding of how to use the Libfabric library for RDMA operations.
- Understanding of how to configure and use the IBverbs, sockets provider, and EFA provider libraries.
- Familiarity with writing and using test cases, optimizing them to ensure code reliability and performance.

### Complexity

- The Libfabric API is complex and has a high learning curve.
- Different network protocols have their own characteristics, resulting in a broad scope of work.
- When using different network protocols, the specific Libfabric operations required are not completely the same, and it is necessary to handle these differences properly.
- Performance optimization requires selecting the optimal solution based on many different application scenarios to ensure performance.
- Guaranteeing reliability requires careful consideration of error handling when modifying the code to support multiple different network protocols and RDMA adapters.

### Related Work

Open MPI uses the communication interface provided by Libfabric to allow applications to switch between different transport layers. When using network protocols supported by ibvers, Libfabric uses IBverbs as the underlying transport protocol for data transfer, thus providing high performance and low-latency communication. When using network protocols not supported by ibvers, other Libfabric providers are used for RDMA operations.

### Project Proposal

Libfabric is an advanced networking library that abstracts the underlying network via adapter endpoints and provides a uniform programming interface for different network protocols to enable high-performance and low-latency RDMA operations. Similar to Open MPI, we can make use of the Libfabric API directly in programs and establish a bridge between the program and hardware through the Libfabric provider, enabling communication via the Libfabric API with different hardware and software platforms.

We need to refactor the existing code that uses IBverbs interface for network communication, replacing it with the Libfabric library and utilizing the unified interface provided by Libfabric to achieve the previous functionality. Additionally, we need to introduce new Libfabric APIs to implement features and functionality that IBverbs does not have.

For different network environments, we need to write code that allows the client and server to perform different initialization configurations and establish connections with each other based on the current network protocol in use. Libfabric selects an appropriate provider, such as verbs, sockets, PAMI, etc., as the underlying network transport layer to complete subsequent remote memory access operations.

The end result is that the client and server select the optimal underlying network transport layer as the provider based on the currently supported network protocol and undergo appropriate initialization configuration before establishing a connection. The client and server then utilize the Libfabric interface to complete RDMA operations via the underlying network transport layer and release resources before exiting.

We can still utilize the existing code framework to accomplish the above operations by simply replacing the previously used IBverbs interface with the Libfabric interface.

The following are the main code refactoring tasks:

| Existing Code                                    | Original Function                                            | Major Changes                                                | Possible API Used                                            |
| ------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Address class                                    | Represents the IP address and port of the RDMA connection.   | Modify the variable used and initialize the necessary parameters. Use the APIs in [fi_av_set](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_av_set.3.html) and [fi_av](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_av.3.html) to manage IP addresses and ports. | `fi_av_open`, `fi_close`<br>`fi_av_bind`, `fi_av_insert` <br>`fi_av_set`, `fi_close`... |
| RDMAActive class and RDMAPassive class           | Actively connect to another node. <br>Passively receive connection requests from other nodes. | Merge into a single Connection class (see table below).      | N/A                                                          |
| Connection class                                 | Conducts RDMA communication, including initialization, message publishing/receiving, event polling, closing connections, batch receiving, publishing writes, and atomic operations. | Renamed to Operations class. <br>Remove initialization and connection closing operations. <br>Modify the variable used and initialize the necessary parameters. <br>Use the APIs in [fi_tagged](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_tagged.3.html) and [fi_msg](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_msg.3.html) to publish buffers for incoming messages and initiate sending messages. Use the APIs in [fi_rma](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_rma.3.html) to complete remote memory read and write operations. Use the APIs in [fi_poll](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_poll.3.html) to complete event polling. Use the APIs in [fi_atomic](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_atomic.3.html) to complete atomic operations on remote memory. | `fi_trecv`, `fi_tsend` <br>`fi_recv`, `fi_recvv`<br>`fi_send`, `fi_sendv`<br>`fi_read`<br>`fi_write`, `fi_readv`<br>`fi_writev`, `fi_poll_open`<br>`fi_poll`, `fi_wait`<br>`fi_atomicv`<br>`fi_fetch_atomic`... |
| Buffer class                                     | Manages the registration and deregistration of local and remote memory. | Modify the variable used and initialize the necessary parameters. <br>Use the APIs in [fi_mr](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_mr.3.html) to manage local and remote memory. | `fi_mr_reg`, `fi_close`<br>`fi_mr_key`, `fi_mr_raw_attr`<br>`fi_mr_refresh`... |
| recv_buffer structure                            | Represents the receive buffer used with IBverbs.             | Modify the variable used and add parameters. <br>Change to a buffer structure that is compatible with Libfabric and use the corresponding buffer type and operations based on the currently used Libfabric provider. For example: unnamed socket buffer, Shared Receive Queue buffer, etc. | `socket()`, `fi_recvmsg()`<br>`fi_sendmsg()`, `fi_srq_open()`<br>`fi_srq_set_attr()`... |
| executor remote function call executor           | Loads shared libraries, allocates and releases resources, receives and sends data. | Modify the variable used. <br>Use the APIs in [fi_eq](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_eq.3.html) and [fi_cntr](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_cntr.3.html) to load shared libraries. Access Libfabric resources through the Fabric class operations. Use the Connect class operations to implement data read and write operations. | `fi_eq_open`, `fi_close`<br>`fi_eq_write`, `fi_eq_read`<br>`fi_cntr_open`, `fi_cntr_add`, `fi_writedata`<br>`fi_readv`... |
| fast_executor function computing service program | Continuously polls the queue, executes function calculations, and returns results to the client. | Modify the variable used. <br> Use the APIs in [fi_cq](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_cq.3.html), [fi_eq](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_eq.3.html), [fi_cntr](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_cntr.3.html), and [fi_poll](https://ofiwg.github.io/Libfabric/v1.17.1/man/fi_poll.3.html) to poll the events queue. Use the Connect class operations to implement data read and write operations. | N/A                                                          |

Aside from the above code, some other code also needs to be modified, such as the server and client code. In this code, we simply use Libfabric APIs and variables instead of the ones previously used. Note that different providers have different resource control capabilities, and hence we use different Libfabric APIs depending on the provider used.

Here is the optimized markdown proposal for submission to Google's GSoC program:

## Possible Code Additions

| New Code           | Functionality                                                | Main Implementation Approach                                 | Possible API Used                                            |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `Fabric` Class     | Stores information such as the supported network protocols, available providers, selected adaptors, Fabric object, domain object, and endpoint object.<br />Retrieves the `fi_info` structure and the saved information inside it.<br />Creates, closes, and manages Fabric, domain, endpoint objects, as well as other related operations. | Uses APIs in `fi_getinfo` to retrieve interface information.<br />Uses APIs in `fi_fabric` to create and close Fabric objects, etc.<br />Uses APIs in `fi_domain` to create and close domain objects.<br />Uses APIs in `fi_endpoint` to create and close endpoints.<br /> | `fi_getinfo`, `fi_freeinfo`<br />`fi_fabric`, `fi_close`<br />`fi_domain`, `fi_domain_bind`<br />`fi_endpoint`, `fi_ep_bind`<br />`fi_enable`, etc. |
| `Connection` Class | Stores information required for establishing a connection.<br />Manages the endpoint connection state.<br />Allows an endpoint to actively connect to another node.<br />Allows an endpoint to listen for connections from other endpoints and passively receive connections from other endpoints.<br />Returns information about the endpoint and connection. | Stores required struct and variable information and initializes parameters.<br />Uses APIs in `fi_cm` to establish connections and return endpoint and connection information. | `fi_connect`, `fi_listen`<br />`fi_accept`, `fi_shutdown`<br />`fi_getname`, etc. |
| `errno` Error      | Converts Fabric errors to printable strings.                 | Uses APIs in `fi_errno`.                                     | `fi_strerror`                                                |

### Timeline

| Week  | Dates                    | Work                                                         |
| ----- | ------------------------ | ------------------------------------------------------------ |
| 0     | Before May 29            | Get familiar with the project, continue contributing to the project via pull requests, read [Libfabric man pages](https://ofiwg.github.io/Libfabric/v1.17.1/man/) to gain mastery over the usage of Libfabric API, refine the project framework and code implementation, and understand the principles and implementation of different network protocols and required Libfabric providers. Prepare for writing code. |
| 1-2   | May 29th - June 11th     | Introduce the Libfabric library and complete preliminary coding of Fabric and Address classes. |
| 3-4   | June 12th - June 25th    | Complete preliminary coding of Buffer and Connection classes. |
| 5-6   | June 26th - July 9th     | Complete initial coding of `recv_buffer` struct, executor remote function call executer, and `fast_executor` function computing service program. |
| 7-8   | July 10th - July 23th    | Complete preliminary coding and modifications of server, client, and other related code. |
| 9-10  | July 24th - August 6th   | Perform preliminary improvement of the overall code to ensure normal program operation when using `verbs` as provider. Conduct testing and code optimization to ensure program reliability and performance. |
| 11-12 | August 7th - August 20th | Finish the overall code to ensure that the program runs properly when using `sockets`, `efa` (for TCP and AWS EFI), and other providers. conduct testing and code optimization to ensure program reliability and performance. |

#### Buffer Time (8 days)

Allow for a buffer period of seven days as a safety net, in case anything does not go as planned.

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

+ Familiarity with common data structures and algorithm problems.[code link](https://github.com/Origin-yy/Code_c/tree/main/Group_Task_C/My_ls)
+ Independently implemented the "ls" command using C language and Linux system calls, supporting flexible combinations of parameters such as -a, -l, -s, -t, -r, -i, and -R.[code link](https://github.com/Origin-yy/Code_c/tree/main/Group_Task_C/My_ls)
+ Independently implemented a shell using C language, inter-process communication and Linux system calls, supporting operations such as pipes, redirects, background running, and changing directory.[code link](https://github.com/Origin-yy/Code_c/tree/main/Group_Task_C/My_Shell)
+ implemented producer-consumer and dining philosophers problems using C multi-threading, as well as a thread pool.[code link](https://github.com/Origin-yy/Code_c/tree/main/Group_Task_C/Thread)
+ Independently developed a C++ multi-user network chat room on a LAN using socket, C++thread pool, I/O multiplexing, and redis. The clients and servers are highly stable and concurrent, supporting common operations such as group chat, adding/deleting friends, file sending, and offline messaging, with data persistence using redis.[code link](https://github.com/Origin-yy/Code_cc/tree/main/Chatroom)

## Others

Why did you choose this project? What made you interested in our organization?

rFaaS supports the high-performance, low-latency calling of network resources using RDMA. The relevant technology required for this project aligns with my learning direction, and I have a strong interest in it.

What do you wish to accomplish during GSoC?

I hope to learn new technologies that I'm interested in, improve my technical and communication collaboration abilities, and make contributions to open-source projects that I am passionate about.

Have you worked with the required technologies before? If yes, then in what scope? Which projects?

I have not used IBverbs, Libfabric, or RDMA before. However, I have some knowledge of network programming, TCP/IP, C++, and thread pools through learning, which I gained in XiYou Linux Group, and can be found on my GitHub.

Did you contribute to open-source projects before? Have you ever made a pull request, helped to fix an issue, or developed a project?

No, this is my first time participating in an open source project.
