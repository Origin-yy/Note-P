## 方案

### 方案一：

任务 1：重构项目为两个兼容实现（使用 ibverbs 和 libfabric）

1. 首先，评估现有项目的架构，了解其核心功能和关键组件。这将有助于确定如何将项目重构为两个兼容实现。
2. 将现有的 ibverbs 实现作为基础，创建一个新的模块或包，将与 ibverbs 相关的代码移到这个模块中。确保模块化设计，以便在需要时可以轻松切换到其他通信库。
3. 为 libfabric 实现创建一个新的模块或包，并在其中实现与 ibverbs 相似的功能。阅读 libfabric 文档以了解如何使用其 API 完成类似的通信任务。
4. 为两个实现定义统一的接口，以便在不同网络环境下可以轻松切换。这也有助于在项目中使用其他通信库时保持一致性。
5. 在项目的主要部分中，根据需要调用这些通用接口。根据项目的配置或运行时参数选择使用 ibverbs 或 libfabric 实现。
6. 编写测试用例，确保两个实现在不同网络环境下均能正常工作。在各种场景下进行性能测试，以便对比两种实现的优劣。

任务 2：通过调整 libfabric 配置支持更多网络协议（如 TCP 和 AWS EFA）

1. 阅读 libfabric 文档，了解如何配置 libfabric 以支持不同的网络协议。特别关注与 TCP 和 AWS EFA 相关的内容。
2. 在 libfabric 实现的模块或包中，添加新的配置选项，以便用户可以选择所需的网络协议。这可能包括选择 libfabric 提供的不同"provider"或调整 libfabric 的初始化参数。 在 libfabric 中
3. 为支持新网络协议的 libfabric 实现编写适当的初始化和清理代码。确保在不同网络协议下都能正常工作。
4. 根据支持的网络协议更新项目文档，向用户提供详细的配置和使用说明。
5. 编写测试用例，确保在 TCP 和 AWS EFA 等网络上的 libfabric 实现能正常工作。进行性能测试，以评估不同网络协议对项目性能的影响。

通过以上步骤，您将能够将项目重构为两个兼容实现（使用 ibverbs 和 libfabric），并通过调整 libfabric 配置以支持更多网络协议（如 TCP 和 AWS EFA）。在实施过程中，请务必关注代码质量、可维护性和性能。

### 方案二

针对这两个任务，我们可以采取以下详细的方案来实现：

任务1：将项目重构为两个使用 ibverbs 或 libfabric 的兼容实现

1. 抽象出通信层接口：首先，根据项目中使用到的ibverbs功能，定义一个通信层接口，该接口应包含所有必要的操作，如连接建立、数据传输、资源管理等。这个接口将充当ibverbs和libfabric之间的中间层，为上层应用提供统一的API。
2. 实现ibverbs和libfabric的适配器：针对通信层接口，分别实现两个适配器类，一个基于ibverbs，另一个基于libfabric。这两个适配器需要实现通信层接口中定义的所有操作，并将这些操作映射到对应的ibverbs或libfabric函数。确保适配器的实现能够处理不同的网络配置和传输特性。
3. 依赖注入：在项目的初始化阶段，根据用户配置或环境变量，选择适当的适配器实现（ibverbs或libfabric），并将其传递给上层应用。上层应用将使用通信层接口与适配器进行交互，而不是直接与ibverbs或libfabric打交道。

任务2：通过调整TCP或AWS EFA等协议的libfabric配置来支持更多种类的网络

1. 在libfabric适配器中添加配置选项：根据libfabric的支持，为适配器添加新的配置选项，以便用户可以选择所需的网络协议（如TCP、AWS EFA等）。例如，可以在适配器的构造函数中加入一个参数，用于指定网络协议。
2. 修改资源分配和连接建立过程：根据用户选择的网络协议，调整适配器中资源分配和连接建立的实现。这可能包括使用不同的域、端点类型、地址向量等。确保每种网络协议的实现都能正确处理连接建立和数据传输。
3. 在适配器中处理协议特性：针对不同的网络协议，处理其特性和限制。例如，针对TCP协议，可能需要处理流控制和拥塞控制；针对AWS EFA，可能需要处理特定的内存注册和数据传输方式。
4. 测试和性能评估：为两个任务编写测试样例，确保在不同网络协议下代码能够正确运行。此外，进行性能测试，评估不同网络协议下的吞吐量、延迟等指标。根据性能测试结果，优化适配器的实现以提高性能。

总结：通过定义通信层接口、实现ibverbs和libfabric的适配器，使用依赖注入选择适当的实现，以及调整libfabric配置来支持更多网络协议，可以实现项目的重构和扩展。在实施过程中，还需要编写测试样例和进行性能评估，以确保代码的正确性和性能。

## benchmarks

### cold_benchmark.cpp

以上代码主要使用了rdmalib库，该库封装了InfiniBand Verbs（ibverbs）库，使其更易于使用。具体使用ibverbs的地方包括： 

1. 在executor对象的构造函数中，使用了rdmalib库中的函数进行了RDMA设备的初始化和连接的建立，其中包括了使用ibverbs库中的函数注册内存区域、创建QP等操作。 
2. 在main函数中，使用了rdmalib库中的函数创建了输入和输出的Buffer对象，并使用ibverbs库中的函数注册了内存区域。
3. 在main函数中，使用了rdmalib库中的函数执行了RDMA操作，其中包括了使用ibverbs库中的函数提交发送和接收请求、等待RDMA操作完成等操作。 总之，rdmalib库封装了ibverbs库中的复杂操作，使得使用者可以更加方便地进行RDMA编程。

### cpp_interface.cpp

这段代码是一个使用RDMA进行并行远程函数调用（RFC）的示例，主要使用了rdmalib、rfaas和ibverbs库。它首先解析命令行参数，读取设备和连接细节，并初始化RDMA连接。然后，它创建多个输入和输出缓冲区，使用IBV_ACCESS_LOCAL_WRITE和IBV_ACCESS_REMOTE_WRITE注册内存，并将其传递给rfaas::executor对象的execute方法，以便并行执行RFC。它还使用rdmalib::Benchmarker类测量RFC的性能指标，如平均执行时间和中位数执行时间。最后，它释放分配的内存并退出。 在并行RFC过程中，主机通过RDMA发送每个输入缓冲区的地址和长度以及输出缓冲区的地址和长度到远程主机。远程主机使用这些信息将结果写入输出缓冲区。本地主机可以使用IBV_WR_RDMA_READ和IBV_WR_RDMA_WRITE操作来读取和写入远程缓冲区。 此示例还演示了如何在并行RFC执行期间使用rdmalib::Benchmarker类来测量执行时间，并如何使用std::tuple来返回两个值的结果。

### parallel_invocations.cpp

这段代码是一个基于RDMA（Remote Direct Memory Access）技术的无服务器计算测试程序。主要使用了ibverbs库来实现RDMA通信。代码中通过读取配置文件等方式获取设备、连接和基准测试等参数，然后使用RDMA协议进行数据传输和计算操作，并使用Benchmarker来测量各个操作的性能。最后输出测试结果和计算结果。

### warm_benchmark.cpp

这段代码是一个使用ibverbs库实现基于RDMA的服务器端测试工具的C++程序。 程序首先使用cxxopts库解析命令行参数，并使用spdlog库设置日志级别和格式。然后，程序从文件中读取设备信息和基准测试设置。接下来，程序从一个数据库文件中读取与执行器的连接详细信息，并创建一个执行器对象，使用RDMA连接到远程节点。执行器对象用于在远程节点上分配和释放内存缓冲区，并使用RDMA传递输入和输出缓冲区来执行用户指定的函数。 程序接着进行预热重复操作，以确保基准测试结果的一致性，然后使用rdmalib::Benchmarker类执行实际的基准测试重复操作，该类用于测量用户指定的函数在远程节点上的执行时间。最后，程序打印基准测试结果，并在指定的情况下将其导出到CSV文件中。 总之，该代码展示了如何使用ibverbs库在两个通过IB网络连接的节点之间执行RDMA操作，以及如何使用rdmalib库管理内存缓冲区并测量用户指定函数在远程节点上的执行时间。

## examples

### image-recognition/client.cpp

该代码是一个使用RDMA技术实现的Serverless函数的测试程序，主要使用了RDMA库rdmalib和Serverless函数执行库rfaas。以下是主要步骤的解释： 1. 读取命令行参数，包括服务器配置、函数名称、输入图片路径等信息。 2. 初始化spdlog日志库，根据verbose参数设置日志级别。 3. 读取服务器配置文件，建立与服务器的连接。 4. 从文件中读取输入图片数据，并使用rfaas库的executor对象分配函数执行所需的资源，包括函数库、核心数、输入数据大小、超时时间等。 5. 使用rdmalib库的Buffer对象创建输入和输出缓冲区，并将其注册到RDMA设备中。 6. 进行函数执行的预热操作，即多次执行函数以预热缓存和连接等，预热次数由warmup_iters参数指定。 7. 进行函数执行的实际测试，即多次执行函数并记录执行时间，执行次数由repetitions参数指定。 8. 输出测试结果，包括平均执行时间、中位数执行时间等，并将执行时间保存到CSV文件中。 9. 释放资源，关闭与服务器的连接。 10. 将函数执行结果输出到文件中。 总体来说，该程序通过RDMA技术实现了Serverless函数的远程执行，可以测试函数执行的性能并输出测试结果。

### thumbnailer/client.cpp

该代码实现了一个基于RDMA的无服务器计算应用，通过RDMA实现了远程内存访问和数据传输，以及对远程函数的调用。 在该代码中，首先读取了配置文件中的服务器信息，然后读取了一个二进制文件，并通过RDMA将其发送到远程服务器中进行计算。在发送前，将输入数据和输出数据都注册到了本地内存中，并分别设置了本地内存访问权限。 在进行实际的计算前，通过执行多次的“热身”操作，使得RDMA连接和远程函数调用都得到了充分的准备和优化。在进行实际计算时，通过RDMA发送输入数据，调用远程函数，然后接收输出数据。在多次计算的过程中，通过Benchmarker统计每次计算的时间，并输出结果。 该代码的主要难点在于对RDMA的使用，需要对RDMA的相关概念和API有一定的了解。同时，该代码还使用了spdlog和cxxopts等库，需要对这些库的使用方法进行了解。

## rdmalib/lib

### buffer.cpp

该代码实现了一个基于RDMA的Buffer类，用于管理本地和远程内存的注册和注销。Buffer类封装了ibv_mr结构体，提供了内存大小、地址、lkey、rkey等信息的查询，并提供了ScatterGatherElement类用于构造Scatter/Gather操作的元素。 在Buffer类中，通过mmap函数和munmap函数实现了本地内存的分配和释放，通过ibv_reg_mr函数和ibv_dereg_mr函数实现了对内存的注册和注销。在注册内存时，需要传入一个ibv_pd结构体，用于表示内存所属的Protection Domain。 在RemoteBuffer类中，封装了远程内存的地址、rkey和大小等信息，用于构造远程读写操作。在进行远程读写时，需要构造一个Scatter/Gather操作的元素，该元素包含了远程内存的地址、大小和rkey等信息。该元素可以用于构造ibv_send_wr结构体，从而实现远程读写操作。 该代码的主要难点在于对RDMA的使用，需要对RDMA的相关概念和API有一定的了解。同时，该代码还使用了spdlog和fmt等库，需要对这些库的使用方法进行了解。

### connection.cpp

这段代码是一个使用ibverbs库实现的C++ Connection类，用于远程直接内存访问（RDMA）通信。 Connection类提供了初始化、发布发送和接收操作、轮询完成事件以及关闭连接的方法。该类还包括支持批量接收操作的功能，以及发布写入和原子操作的功能。 代码使用ibverbs库与InfiniBand硬件交互。ibverbs库提供了与InfiniBand硬件交互的低级接口，包括创建和配置队列、发布和轮询完成事件，以及执行RDMA操作。 代码使用spdlog库进行日志记录。spdlog库提供了一种简单高效的日志记录机制，可以轻松地集成到C++应用程序中。 总的来说，这段代码是一个结构良好且高效的Connection类实现，使用ibverbs库进行RDMA通信。

### rdmalib.cpp

是一个C++封装的RDMA库，使用了ibverbs和rdma_cm库。以下是每个类和函数的主要作用和实现： 

1. Address类 该类用于表示RDMA连接的地址，包括IP和端口。主要成员函数有：
   - Address()：构造函数，初始化Address类的成员变量。 
   - Address(const std::string & ip, int port, bool passive)：构造函数，初始化Address类的成员变量，包括hints、addrinfo等。 
   - Address(const std::string & sip, const std::string & dip, int port)：构造函数，用于创建一个连接到另一个节点的地址。 
   - ~Address()：析构函数，释放addrinfo。
2. RDMAActive类 该类用于主动连接到另一个节点。主要成员函数有： 
   - RDMAActive()：构造函数，初始化RDMAActive类的成员变量。 
   - RDMAActive(const std::string & ip, int port, int recv_buf, int max_inline_data)：构造函数，初始化RDMAActive类的成员变量，包括_cfg、_addr等。
   - allocate()：为当前对象分配Connection对象。 
   - connect(uint32_t secret)：连接到远程节点。 
   - disconnect()：断开连接。 
   - pd() const：获取Protection Domain（PD）。 
   - connection()：获取Connection对象。 
   - is_connected()：判断当前是否连接到远程节点。
3. RDMAPassive类 该类用于被动接收来自其他节点的连接请求。主要成员函数有： 
   - RDMAPassive(const std::string & ip, int port, int recv_buf, bool initialize, int max_inline_data)：构造函数，初始化RDMAPassive类的成员变量，包括_cfg、_addr等。 
   - ~RDMAPassive()：析构函数，销毁listen_id和event_channel。
   -  allocate()：为当前对象分配Connection对象。 
   - pd() const：获取Protection Domain（PD）。 
   - set_nonblocking_poll()：设置RDMA event channel为非阻塞模式。 
   - nonblocking_poll_events(int timeout)：使用poll函数轮询RDMA event channel上的事件。 
   - poll_events(bool share_cqs)：等待RDMA事件，并根据事件类型执行相应操作。 
   - accept(Connection* connection)：接受连接请求。
4. Connection类 该类用于表示RDMA连接。主要成员函数有： 
   - Connection(bool is_listener = false)：构造函数，初始化Connection类的成员变量，包括verbs、id、qp等。 - 
   - initialize(rdma_cm_id* id)：初始化Connection对象。 
   - close()：关闭Connection对象。 
   - set_private_data(uint32_t secret)：设置Connection对象的私有数据。 
   - get_private_data() const：获取Connection对象的私有数据。 
   - qp() const：获取Queue Pair（QP）对象。 
   - id() const：获取RDMA连接标识符。
5. impl命名空间 该命名空间包含一些用于错误检查和调试的辅助函数。 以上是该RDMA库的主要类和函数，其中RDMAActive和RDMAPassive类分别用于主动连接和被动接收连接，Connection类表示一个RDMA连接，Address类表示连接的地址。整个库使用了C++11的特性，使用了智能指针和异常处理等机制。

## rdmalib/include/rdmalib

### buffer.hpp

这段代码是一个C++的头文件，定义了使用InfiniBand Verbs（ibverbs）库进行RDMA通信时所需的缓冲区结构体和相关操作。具体来说，它包括以下内容： 

1. `Buffer`和`RemoteBuffer`结构体：分别表示本地和远程缓冲区，其中`Buffer`是一个模板类，可以用于存储任何类型的数据，而`RemoteBuffer`则是一个简单的结构体，包含远程缓冲区的地址、访问权限和大小等信息。
2. `ScatterGatherElement`结构体：表示分散-聚集（scatter-gather）操作中的元素，即将数据从一个或多个源缓冲区发送到一个或多个目标缓冲区的操作。这个结构体包含一个可变长度的`std::vector`，用于存储一个或多个`ibv_sge`结构体，表示数据在物理内存中的位置、大小和访问权限等信息。
3. `impl::Buffer`结构体：是`Buffer`结构体的实现类，包含本地缓冲区的指针、大小、字节数、内存区域的访问权限和相关的RDMA操作等方法。
4. 一些辅助方法，如数据序列化和反序列化的方法等。 总体来说，这个头文件提供了一些基本的RDMA操作和数据结构，可以用于构建更高级的RDMA应用程序。

### connection.hpp

以上代码定义了一个 C++ 类 `Connection`，它使用 RDMA Connection Manager (rdma_cm) 和 InfiniBand Verbs (ibverbs) 库来管理 RDMA 连接。 该类的主要功能包括： - 管理 RDMA Connection Manager 标识符 (`rdma_cm_id`) 和与连接相关的 InfiniBand Queue Pair (`ibv_qp`)。 - 提供了将不同类型的 RDMA 操作（发送、接收、写、比较交换、原子加）提交到队列对的发送和/或接收队列的方法。 - 可以轮询与队列对关联的完成队列中的完成事件。 - 可以使用完成通道注册以接收事件通知。 - 提供了一些便捷方法来初始化队列对并批量提交操作。 该代码还定义了一个 `ConnectionConfiguration` 结构体，它封装了队列对和连接建立的配置参数。 该代码使用了 C++ 标准库类型，如 `std::array`、`std::tuple`、`std::initializer_list` 和 `std::vector`，以及来自 ibverbs 和 rdma_cm 库的类型，如 `ibv_wc`、`ibv_send_wr`、`rdma_conn_param`、`rdma_cm_id`、`ibv_qp_init_attr`、`ibv_comp_channel` 和 `ibv_cq`。 该代码使用枚举类型来表示队列的类型（SEND 或 RECV）和连接的状态（UNKNOWN、REQUESTED、ESTABLISHED 或 DISCONNECTED）。该类还具有用于管理连接状态和完成事件的私有数据成员。

### rdmalib.hpp

给定的代码是一个C++头文件，定义了一组类和结构体，用于使用ibverbs库处理RDMA（Remote Direct Memory Access）。 该头文件定义了两个主要结构体：`Address`和`Connection`。`Address`结构体表示网络地址，包含诸如IP地址和端口号等信息。`Connection`结构体表示RDMA连接，包含诸如队列对（QP）、保护域（PD）和内存区域（MR）等信息。 该头文件还定义了两个高级类：`RDMAActive`和`RDMAPassive`。`RDMAActive`类表示主动RDMA端点，用于发起连接，而`RDMAPassive`类表示被动RDMA端点，用于接受传入的连接。这两个类都包含`Address`和`Connection`的实例，以及其他与RDMA相关的对象，例如事件通道和完成队列。 `RDMAActive`类提供了连接到远程端点、断开连接和分配内存资源的方法。`RDMAPassive`类提供了接受传入连接、轮询新事件和分配内存资源的方法。 总体而言，该头文件提供了一个高级接口，用于使用ibverbs库处理RDMA，抽象了RDMA协议的许多低级细节。

### recv_buffer.hpp

以上代码定义了一个 C++ 结构体 `RecvBuffer`，用于表示使用 InfiniBand Verbs API (`ibverbs`) 的 RDMA 连接的接收缓冲区。 该结构体包含以下成员：

- `_rcv_buf_size`：一个整数，表示接收缓冲区的大小。 
- `_refill_threshold`：一个整数，表示缓冲区在需要重新填充之前必须保持的最小可用接收工作请求数。 
- `_requests`：一个整数，表示当前待处理的接收工作请求数。
-  `DEFAULT_REFILL_THRESHOLD`：一个常量整数，表示 `_refill_threshold` 的默认值。 
- `_conn`：指向 `rdmalib::Connection` 对象的指针，表示与接收缓冲区相关联的 RDMA 连接。 该结构体提供以下成员函数： 
- `connect()`：一个函数，将指向 `rdmalib::Connection` 对象的指针作为参数，并将其设置为与接收缓冲区相关联的连接。它还重置待处理的接收工作请求数，并重新填充缓冲区。 
- `poll()`：一个函数，轮询与连接相关联的接收队列的完成队列，并返回一个元组，其中包含指向 `ibv_wc` 结构体数组的指针和已完成的工作请求数。`blocking` 参数可用于指定函数是否应阻塞，直到至少有一个工作请求完成。 
- `refill()`：一个函数，如果待处理的工作请求数低于重新填充阈值，则使用空接收工作请求重新填充接收缓冲区。如果成功重新填充，则函数返回 `true`，否则返回 `false`。 总体而言，该代码提供了一个简单的接口，用于使用 `ibverbs` API 管理 RDMA 连接的接收缓冲区。`connect()` 函数初始化缓冲区，`poll()` 函数可用于检查已完成的工作请求，而 `refill()` 函数则确保始终有足够的待处理工作请求，以避免连接停滞。

## rfaas/lib

### connection.cpp

这段代码是一个使用ibverbs库的C++代码，主要用于管理RDMA连接。代码中包含了对ibverbs库的头文件，同时使用了rdmalib和rfaas库。其中，rdmalib库提供了一些RDMA相关的函数和工具，而rfaas库则是一个基于RDMA的函数即服务（Function-as-a-Service）框架。 在代码中，manager_connection类是一个用于管理RDMA连接的类。在构造函数中，它会初始化一些成员变量，包括连接地址、端口号、接收缓冲区大小、最大内联数据大小等。在connect()函数中，它会连接到指定的地址和端口，并初始化一些RDMA相关的资源，如接收缓冲区和批量接收工作完成（WC）。在disconnect()函数中，它会发送一个释放请求，并在连接断开时断开连接。在submit()函数中，它会将一个分散收集元素（sge）发送到连接中，以提交一个RDMA操作。 整个代码的功能是实现了一个基于RDMA的函数即服务（Function-as-a-Service）框架中的RDMA连接管理器。

### executor.cpp

该代码实现了一个基于InfiniBand RDMA技术的远程函数调用(RPC)执行器(executor)，用于在远程主机上执行函数。主要使用了ibverbs库提供的功能。代码中包含了加载共享库、分配和释放资源、接收和发送数据等功能的实现。其中，加载共享库的函数实现了动态链接库的加载和函数名称的提取，以便后续发送函数调用请求时使用。分配资源的函数实现了与远程主机的连接和数据传输的初始化。释放资源的函数实现了与远程主机的断开连接、清理资源等操作。接收和发送数据的函数实现了数据的接收和发送，并通过多线程处理异步请求。

## server/executor

### server.hpp

这段代码是一个使用ibverbs库实现RDMA通信的C++服务器。服务器使用被动RDMA连接，并提供一组选项以配置服务器的各个方面，例如廉价执行器和快速执行器的数量、接收缓冲区的大小以及最大内联数据大小等。该服务器还具有SignalHandler结构来处理信号，以及Server结构包含各种功能，例如注册缓冲区、监听传入连接和轮询通信等。此外，还有一个FastExecutors结构提供了一个更优化的执行器实现，而当前注释掉的Executors结构。

### server.cpp

这是一个使用ibverbs进行RDMA通信的服务器端代码。它使用了rdmalib库来处理RDMA连接和通信。代码中包含了一个SignalHandler类来处理信号，一个Server类来处理服务器端逻辑。在Server类中，listen()函数用于监听连接请求，poll_communication()函数用于轮询通信事件并返回连接对象，poll_server_notify()函数用于轮询RDMA工作完成事件并通知执行线程，poll_server()函数用于轮询RDMA工作完成事件并处理执行逻辑，poll_threads()函数用于并发地执行执行逻辑。代码中还包含了一些辅助类和函数，例如Buffer类和Connection类等。

## server/executor_manager

### cli.cpp

这段代码是一个C++实现的rFaaS执行器管理器，使用了ibverbs库进行RDMA通信。主要功能是读取配置文件，启动执行器管理器并捕获SIGINT信号。在main函数中，首先调用了ibv_fork_init()函数来初始化ibverbs库，然后读取命令行参数并设置日志级别，接着设置日志格式并输出启动信息。之后，通过sigaction函数注册了一个SIGINT信号处理函数，用于在接收到该信号时调用执行器管理器的shutdown函数。然后读取设备信息和执行器管理器设置，并用这些信息创建一个执行器管理器实例mgr。在实例创建完成后，调用mgr的start函数启动执行器管理器。最后，输出执行器管理器正在关闭的信息，并让线程休眠1秒后退出程序。

### client.cpp

这段代码实现了一个使用ibverbs库的rFaaS（Remote Function as a Service）执行器管理器。主要功能包括： 1. 初始化ibverbs库 2. 解析命令行参数 3. 配置spdlog日志记录器 4. 注册SIGINT信号处理函数 5. 读取设备信息和执行器管理器设置 6. 启动执行器管理器 7. 关闭执行器管理器 具体实现过程如下： 1. 调用ibv_fork_init()函数初始化ibverbs库，并检查初始化是否成功。 2. 使用cxxopts库解析命令行参数，包括verbose标志和配置文件路径等。 3. 配置spdlog日志记录器，设置日志输出级别和格式。 4. 注册SIGINT信号处理函数，以便在程序接收到SIGINT信号时优雅地关闭执行器管理器。 5. 从指定的设备数据库文件中读取设备信息，使用rdmalib库提供的deserialize()函数实现。 6. 从指定的配置文件中读取执行器管理器的设置，使用自定义的Settings类和deserialize()函数实现。 7. 创建一个Manager对象，并调用其start()函数启动执行器管理器。 8. 当程序接收到SIGINT信号时，调用signal_handler()函数，该函数中调用Manager对象的shutdown()函数来关闭执行器管理器。 9. 关闭执行器管理器后，程序等待1秒钟后结束。 需要注意的是，这段代码中有一行全局变量instance = nullptr;，并在Manager对象创建后将其设置为instance = &mgr;。这是为了在signal_handler()函数中调用Manager对象的成员函数时使用的，因为signal_handler()函数是一个全局函数，无法直接访问Manager对象。

### client.hpp

这段代码定义了一个名为Client的结构体，用于封装与执行器之间的通信和状态信息。主要功能包括：

1. 定义了一个名为RECV_BUF_SIZE的常量，表示接收缓冲区的大小。
2. 定义了一个rdmalib::Connection类型的成员变量connection，表示与执行器之间的连接
3. 定义了一个rdmalib::Buffer<rdmalib::AllocationRequest>类型的成员变量allocation_requests，表示分配请求的缓冲区。 
4. 定义了一个rdmalib::RecvBuffer类型的成员变量rcv_buffer，表示接收缓冲区。 
5. 定义了一个std::unique_ptr<ActiveExecutor>类型的成员变量executor，表示当前正在运行的执行器。
6. 定义了一个rdmalib::Buffer<Accounting>类型的成员变量accounting，表示执行器的计费信息。
7. 定义了一个uint32_t类型的成员变量allocation_time，表示分配执行器的时间戳。
8. 定义了一个bool类型的成员变量_active，表示该客户端是否处于活动状态。 该结构体提供了以下成员函数： 
9. 构造函数Client(rdmalib::Connection* conn, ibv_pd* pd)，用于初始化成员变量。 
10. 成员函数reload_queue()，用于重新加载分配请求缓冲区。 
11. 成员函数disable(int)，用于禁用客户端并关闭与执行器的连接。 
12. 成员函数active()，用于检查该客户端是否处于活动状态。 该结构体还使用了rdmalib库中的AllocationRequest、RecvBuffer、Connection等结构体和类，以及自定义的ActiveExecutor和Accounting类，用于实现与执行器之间的通信和计费功能。

### fast_executor.cpp

该代码是一个使用ibverbs实现的服务器应用程序，它通过RDMA（远程直接内存访问）技术提供函数计算服务。主要包括以下几个部分： 1. Thread::work()函数：执行函数计算的主要逻辑，接收客户端发送的请求，调用对应的函数指针进行计算，并将计算结果通过RDMA发送回客户端。 2. Thread::hot()函数：用于热轮询，不断轮询接收队列，当有请求时调用Thread::work()函数进行处理。该函数还处理了一些超时和计数的逻辑。 3. Thread::warm()函数：用于暖轮询，与Thread::hot()类似，但是会在处理完一定数量的请求后切换为热轮询模式。 4. Thread::thread_work()函数：线程的主要逻辑，包括建立RDMA连接，接收客户端发送的函数数据，启动热轮询或暖轮询，执行函数计算，发送计算结果，最后释放资源。 5. FastExecutors类：多线程执行器，负责创建和管理多个Thread线程，并提供一些管理接口，如close()和allocate_threads()。 整个应用程序的核心思想是使用RDMA技术来提供函数计算服务，通过热轮询和暖轮询两种方式来处理客户端请求，以提高服务器的响应速度和处理能力。

# Libfabric:

##OVERVIEW（概述）

Libfabric 是一个高性能结构软件库，旨在为结构硬件提供低延迟接口。Libfabric 为跨结构软件和硬件通信的应用程序软件提供“处理直接 I/O”。进程直接 I/O，历史上称为 RDMA，允许应用程序直接访问网络资源而无需操作系统干预。数据传输可以直接发生在应用程序内存之间。

**libfabric 的两个组件：**

**Fabric Providers**和**Fabric Interfaces**（Fabric 提供组件和 Fabric 接口组件）

+ 从概念上讲，Fabric提供组件可被视为本地硬件 NIC 驱动程序，但 Fabric 提供组件不受此定义的限制。 libfabric  的第一个组件是一个通用框架，能够处理不同类型的结构硬件。所有结构硬件设备及其软件驱动程序都需要支持此框架。插入 libfabric 框架的设备和驱动程序称为 Fabric 提供组件，或简称为提供组件。

+ 第二个组件是一组通信操作。 Libfabric 定义了供应商可以支持的几组通信功能。不要求提供者实现所有定义的接口；但是，提供商会清楚地表明他们支持哪些接口。

##FABRIC INTERFACES（Fabric 接口）

结构接口的设计使其具有内聚性，而不仅仅是不相交接口的结合。这些接口在逻辑上分为两组：**控制接口和通信操作**。

+ 控制接口是一组通用操作，可提供对本地通信资源（如地址向量和事件队列）的访问。

+ 通信操作公开特定的通信模型和结构功能，例如消息队列、远程内存访问和原子操作。通信操作与结构端点相关联。

应用程序通常会使用控制接口来发现本地功能并分配必要的资源。然后，他们将分配和配置一个通信端点以发送和接收数据，或与远程端点执行其他类型的数据传输。   

###CONTROL INTERFACES 控制接口

控制接口 API 为应用程序提供对网络资源的访问。这涉及列出所有可用接口、获取接口的功能并打开提供程序。

+ **fi_getinfo - Fabric 信息**

  fi_getinfo 调用是用于发现和请求系统提供的结构服务的基本调用。应用程序可以使用此调用来指示它们所需的通信类型。 fi_getinfo 的结果 **fi_info** 用于预留和配置结构资源。

  fi_getinfo 返回 fi_info 结构列表。每个结构引用一个 fabric 提供者，指示提供者支持的接口，以及一组命名的资源。fabric 提供者可以在返回的列表中包含多个 fi_info 结构。

+ **fi_fabric - Fabric 域**

  Fabric 域表示访问单个物理或虚拟网络的硬件和软件资源的集合。系统中所有可以通过Fabric相互通信的网口都属于同一个Fabric 域。Fabric 域共享网络地址并且可以跨越多个提供者。 libfabric 支持连接到多个*fabric* 的系统。

+ **fi_domain - Access Domains 访问域**

  访问域表示到 fabric 的单个逻辑连接。它可以映射到单个物理或虚拟 NIC 或端口。访问域定义了可以关联架构资源的边界。每个访问域都属于一个 Fabric 域。

+ **fi_endpoint - Fabric 端点**

  fabric 端点是一个通信门户。端点可以是主动的或被动的。被动端点用于侦听连接请求。主动端点可以执行数据传输。端点配置有特定的通信能力和数据传输接口。

+ **fi_eq - Event Queue 事件队列**

  事件队列，用于收集和报告异步操作和事件的完成情况。事件队列报告与数据传输操作没有直接关联的事件。

+ **fi_cq - Completion Queue 完成队列**

  完成队列是用于报告数据传输操作完成的高性能事件队列。

+ **fi_cntr - Event Counters 事件计数器**

  事件计数器用于报告已完成的异步操作的数量。事件计数器被认为是轻量级的，因为完成只是简单地增加一个计数器，而不是将一个条目放入事件队列中。

+ **fi_mr - Memory Region 内存区域**

  内存区域描述应用程序本地内存缓冲区。为了让结构资源访问应用程序内存，应用程序必须首先通过构建内存区域向结构提供者授予权限。特定类型的数据传输操作需要内存区域，例如 RMA 传输（见下文）。

+ **fi_av - Address Vector 地址向量**

  地址向量用于将更高级别的地址（例如 IP 地址）映射到结构特定地址，这对于应用程序来说可能更自然。地址向量的使用允许提供商减少维护大型地址查找表所需的内存量，并消除数据传输操作期间昂贵的地址解析和查找方法。

### DATA TRANSFER INTERFACES 数据传输接口

结构端点与多个数据传输接口相关联。每个接口集都设计为支持特定的通信方式，端点允许不同的接口结合使用。以下数据传输接口由 libfabric 定义。

+ **fi_msg - Message Queue - 消息队列**

  消息队列向应用程序公开一个简单的、基于消息的 FIFO 队列接口。消息数据传输允许应用程序在保持消息边界的情况下发送和接收数据。

+ **fi_tagged - Tagged Message Queues 标记消息队列**

  标记消息列表公开了基于标记消息传递概念构建的发送/接收数据传输操作。标记消息队列在概念上类似于标准消息队列，但为每条消息添加了 64 位标记。发送的消息与标有相似值的接收缓冲区相匹配。

+ **fi_rma - Remote Memory Access 远程内存访问**

  RMA 传输是一种单向操作，可将数据直接读取或写入远程内存区域。除了定义适当的内存区域外，RMA 操作不需要在目标端进行交互即可完成数据传输。

+ **fi_atomic - Atomic 原子**

  原子操作可以在远程内存区域执行多个操作之一。原子操作包括众所周知的功能，如原子添加和比较和交换，以及其他几个预定义的调用。与其他数据传输接口不同，原子操作知道目标内存区域的数据格式。

### LOGGING INTERFACE 记录接口

可以使用 FI_LOG_LEVEL、FI_LOG_PROV 和 FI_LOG_SUBSYS 环境变量来控制日志记录。

FI_LOG_LEVEL 控制输出的日志数据量。定义了以下日志级别。

- *Warn*：*Warn *是最不详细的设置，用于报告错误或警告。
- *Trace*：Trace 更加冗长，旨在包括有助于跟踪程序执行的非详细输出。
- *Info*：*Info* 是高流量，意味着详细的输出。
- *Debug*：*Debug* 可能会影响应用程序性能。只有在启用调试的情况下编译库时，调试输出才可用。

## fi_atomic 远程原子函数

原子传输用于以原子方式读取和更新位于远程内存区域中的数据。从概念上讲，它们类似于具有相似性质的局部原子操作（例如原子增量、比较和交换等）。对远程数据的更新涉及对数据的几种操作之一，并作用于特定类型的数据，如下所列。因此，原子传输知道被访问数据的格式。单个原子函数可以跨数据数组操作，将原子操作应用于每个条目，但操作的原子性仅限于单个数据类型或条目。

- fi_atomic / fi_atomicv / fi_atomicmsg / fi_inject_atomic

  对远程内存发起原子操作

- fi_fetch_atomic / fi_fetch_atomicv / fi_fetch_atomicmsg

  启动对远程内存的原子操作，检索初始值。

- fi_compare_atomic / fi_compare_atomicv / fi_compare_atomicmsg

  启动对远程内存的原子比较操作，检索初始值。

- fi_atomicvalid / fi_fetch_atomicvalid / fi_compare_atomicvalid / fi_query_atomic

  指示提供者是否支持特定的原子操作

```c
#include <rdma/fi_atomic.h>

ssize_t fi_atomic(struct fid_ep *ep, const void *buf,
	size_t count, void *desc, fi_addr_t dest_addr,
	uint64_t addr, uint64_t key,
	enum fi_datatype datatype, enum fi_op op, void *context);

ssize_t fi_atomicv(struct fid_ep *ep, const struct fi_ioc *iov,
	void **desc, size_t count, fi_addr_t dest_addr,
	uint64_t addr, uint64_t key,
	enum fi_datatype datatype, enum fi_op op, void *context);

ssize_t fi_atomicmsg(struct fid_ep *ep, const struct fi_msg_atomic *msg,
	uint64_t flags);

ssize_t fi_inject_atomic(struct fid_ep *ep, const void *buf,
	size_t count, fi_addr_t dest_addr,
	uint64_t addr, uint64_t key,
	enum fi_datatype datatype, enum fi_op op);

ssize_t fi_fetch_atomic(struct fid_ep *ep, const void *buf,
	size_t count, void *desc, void *result, void *result_desc,
	fi_addr_t dest_addr, uint64_t addr, uint64_t key,
	enum fi_datatype datatype, enum fi_op op, void *context);

ssize_t fi_fetch_atomicv(struct fid_ep *ep, const struct fi_ioc *iov,
	void **desc, size_t count, struct fi_ioc *resultv,
	void **result_desc, size_t result_count, fi_addr_t dest_addr,
	uint64_t addr, uint64_t key, enum fi_datatype datatype,
	enum fi_op op, void *context);

ssize_t fi_fetch_atomicmsg(struct fid_ep *ep,
	const struct fi_msg_atomic *msg, struct fi_ioc *resultv,
	void **result_desc, size_t result_count, uint64_t flags);

ssize_t fi_compare_atomic(struct fid_ep *ep, const void *buf,
	size_t count, void *desc, const void *compare,
	void *compare_desc, void *result, void *result_desc,
	fi_addr_t dest_addr, uint64_t addr, uint64_t key,
	enum fi_datatype datatype, enum fi_op op, void *context);

size_t fi_compare_atomicv(struct fid_ep *ep, const struct fi_ioc *iov,
       void **desc, size_t count, const struct fi_ioc *comparev,
       void **compare_desc, size_t compare_count, struct fi_ioc *resultv,
       void **result_desc, size_t result_count, fi_addr_t dest_addr,
       uint64_t addr, uint64_t key, enum fi_datatype datatype,
       enum fi_op op, void *context);

ssize_t fi_compare_atomicmsg(struct fid_ep *ep,
	const struct fi_msg_atomic *msg, const struct fi_ioc *comparev,
	void **compare_desc, size_t compare_count,
	struct fi_ioc *resultv, void **result_desc, size_t result_count,
	uint64_t flags);

int fi_atomicvalid(struct fid_ep *ep, enum fi_datatype datatype,
    enum fi_op op, size_t *count);

int fi_fetch_atomicvalid(struct fid_ep *ep, enum fi_datatype datatype,
    enum fi_op op, size_t *count);

int fi_compare_atomicvalid(struct fid_ep *ep, enum fi_datatype datatype,
    enum fi_op op, size_t *count);

int fi_query_atomic(struct fid_domain *domain,
    enum fi_datatype datatype, enum fi_op op,
    struct fi_atomic_attr *attr, uint64_t flags);
```

### ARGUMENTS

- *ep*

  在其上启动原子操作的结构端点。

- *buf*

  指定原子操作的第一个操作数的本地数据缓冲区

- *iov / comparev / resultv iov *

  矢量数据缓冲区。

- *count / compare_count / result_count *

  矢量数据条目的计数。引用的元素数，其中每个元素都是指定的数据类型。

- *addr*

  要访问的远程内存地址。

- *key*

  与远程内存关联的保护密钥。

- *datatype*

  与原子操作数关联的数据类型

- *op*

  要执行的原子操作

- *compare*

  本地比较缓冲区，包含比较数据。

- *result*

  本地数据缓冲区，用于存储远程缓冲区的初始值

- *desc / compare_desc / result_desc*

  分别与本地数据缓冲区、本地比较缓冲区和本地结果缓冲区关联的数据描述符。请参见 `fi_mr` (3)。

- *dest_addr*

  无连接原子操作的目标地址。忽略已连接的端点。

- *msg*

  原子操作的消息描述符

- *flags*

  申请原子操作的附加标志

- *context*

  用户指定的指针与操作关联。如果操作不会生成成功完成，则忽略此参数，除非操作标志指定用于所需输入的上下文参数。



