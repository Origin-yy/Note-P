```cmake
###
# pistache
###
find_package(pistache QUIET)
if(NOT pistache_FOUND)
  message(STATUS "Downloading and building pistache dependency")
  FetchContent_Declare(
    pistache
    GIT_REPOSITORY https://github.com/pistacheio/pistache.git
    CMAKE_ARGS -DCXXOPTS_BUILD_EXAMPLES=Off -DCXXOPTS_BUILD_TESTS=Off
  )
  FetchContent_MakeAvailable(pistache)
endif()

###
# readerwriterqueue
###
find_package(readerwriterqueue 1.0.6 EXACT QUIET)
if(NOT readerwriterqueue_FOUND)
  message(STATUS "Downloading and building readerwriterqueue dependency")
  FetchContent_Declare(
    readerwriterqueue
    GIT_REPOSITORY https://github.com/cameron314/readerwriterqueue
    CMAKE_ARGS -DCXXOPTS_BUILD_EXAMPLES=Off -DCXXOPTS_BUILD_TESTS=Off
    GIT_TAG master
  )
  FetchContent_MakeAvailable(readerwriterqueue)
endif()

    
    
    
    
if( NOT READERWRITERQUEUE_PATH STREQUAL "")
  set(readerwriterqueue_DIR "${READERWEITERQUEUES_PATH}/lib/cmake/readerwriterqueue")
endif()
    
    
    
###
# rdmalib: build C++14, PIC and no RTTI
###
file(GLOB rdmalib_files "rdmalib/lib/*.cpp")
add_library(rdmalib STATIC ${rdmalib_files})
add_dependencies(rdmalib readerwriterqueue)
add_dependencies(rdmalib spdlog)
add_dependencies(rdmalib cereal::cereal)
target_include_directories(rdmalib PUBLIC "rdmalib/include")
target_include_directories(rdmalib SYSTEM PUBLIC $<TARGET_PROPERTY:PkgConfig::rdmacm,INTERFACE_INCLUDE_DIRECTORIES>)
target_include_directories(rdmalib SYSTEM PUBLIC $<TARGET_PROPERTY:PkgConfig::ibverbs,INTERFACE_INCLUDE_DIRECTORIES>)
#target_include_directories(rdmalib SYSTEM PUBLIC $<TARGET_PROPERTY:PkgConfig::Pistache,INTERFACE_INCLUDE_DIRECTORIES>)
#target_include_directories(rdmalib SYSTEM PUBLIC $<TARGET_PROPERTY:readerwriterqueue,INTERFACE_INCLUDE_DIRECTORIES>)
target_include_directories(rdmalib SYSTEM PUBLIC $<TARGET_PROPERTY:cereal::cereal,INTERFACE_INCLUDE_DIRECTORIES>)
target_include_directories(rdmalib SYSTEM PRIVATE $<TARGET_PROPERTY:spdlog::spdlog,INTERFACE_INCLUDE_DIRECTORIES>)
set_target_properties(rdmalib PROPERTIES POSITION_INDEPENDENT_CODE On)
set_target_properties(rdmalib PROPERTIES LIBRARY_OUTPUT_DIRECTORY lib)
target_link_libraries(rdmalib PUBLIC PkgConfig::rdmacm)
target_link_libraries(rdmalib PUBLIC PkgConfig::ibverbs)
#target_link_libraries(rdmalib PUBLIC PkgConfig::Pistache)
target_link_libraries(rdmalib PUBLIC readerwriterqueue)
target_link_libraries(rdmalib PRIVATE spdlog::spdlog)
target_link_libraries(rdmalib PRIVATE cereal::cereal)

###
# client library
###
file(GLOB rdmalib_files "rfaas/lib/*.cpp")
add_library(rfaaslib STATIC ${rdmalib_files})
add_dependencies(rfaaslib readerwriterqueue)
add_dependencies(rfaaslib spdlog)
add_dependencies(rfaaslib cereal::cereal)
add_dependencies(rfaaslib rdmalib)
target_include_directories(rfaaslib PUBLIC "rfaas/include")
target_include_directories(rfaaslib PRIVATE $<TARGET_PROPERTY:rdmalib,INTERFACE_INCLUDE_DIRECTORIES>)
target_include_directories(rfaaslib SYSTEM PUBLIC $<TARGET_PROPERTY:cereal::cereal,INTERFACE_INCLUDE_DIRECTORIES>)
target_include_directories(rfaaslib SYSTEM PUBLIC $<TARGET_PROPERTY:spdlog::spdlog,INTERFACE_INCLUDE_DIRECTORIES>)
set_target_properties(rfaaslib PROPERTIES POSITION_INDEPENDENT_CODE On)
set_target_properties(rfaaslib PROPERTIES LIBRARY_OUTPUT_DIRECTORY lib)
target_link_libraries(rfaaslib PUBLIC rdmalib)
target_link_libraries(rfaaslib PUBLIC PkgConfig::rdmacm)
target_link_libraries(rfaaslib PUBLIC PkgConfig::ibverbs)
target_link_libraries(rfaaslib PUBLIC pistache)
target_link_libraries(rfaaslib PUBLIC readerwriterqueue)
target_link_libraries(rfaaslib PUBLIC spdlog::spdlog)
target_link_libraries(rfaaslib PRIVATE cereal::cereal)
target_link_libraries(rfaaslib PUBLIC dl)
target_link_libraries(rfaaslib PUBLIC Threads::Threads)

###
# Server
###
add_executable(executor
  server/executor/cli.cpp
  server/executor/opts.cpp
  server/executor/fast_executor.cpp
  server/executor/functions.cpp
)
add_executable(executor_manager
  server/executor_manager/cli.cpp
  server/executor_manager/opts.cpp
  server/executor_manager/settings.cpp
  server/executor_manager/manager.cpp
  server/executor_manager/client.cpp
  server/executor_manager/executor_process.cpp
)
add_executable(resource_manager
  server/resource_manager/cli.cpp
  server/resource_manager/opts.cpp
  server/resource_manager/db.cpp
  server/resource_manager/http.cpp
  server/resource_manager/settings.cpp
  server/resource_manager/manager.cpp
)
set(targets "executor" "executor_manager" "resource_manager")
foreach(target ${targets})
  add_dependencies(${target} readerwriterqueue)
  add_dependencies(${target} cxxopts::cxxopts)
  add_dependencies(${target} spdlog::spdlog)
  add_dependencies(${target} rdmalib)
  add_dependencies(${target} rfaaslib)
  target_include_directories(${target} PRIVATE server/)
  target_include_directories(${target} PRIVATE $<TARGET_PROPERTY:rfaaslib,INTERFACE_INCLUDE_DIRECTORIES>)
  target_include_directories(${target} PRIVATE $<TARGET_PROPERTY:rdmalib,INTERFACE_INCLUDE_DIRECTORIES>)
  target_include_directories(${target} SYSTEM PRIVATE $<TARGET_PROPERTY:readerwriterqueue,INTERFACE_INCLUDE_DIRECTORIES>)
  target_include_directories(${target} SYSTEM PRIVATE $<TARGET_PROPERTY:cxxopts::cxxopts,INTERFACE_INCLUDE_DIRECTORIES>)
  target_link_libraries(${target} PRIVATE readerwriterqueue)
  target_link_libraries(${target} PRIVATE spdlog::spdlog)
  target_link_libraries(${target} PRIVATE rdmalib)
  target_link_libraries(${target} PRIVATE rfaaslib)
  target_link_libraries(${target} PRIVATE Threads::Threads)
  set_target_properties(${target} PROPERTIES RUNTIME_OUTPUT_DIRECTORY bin)
endforeach()
target_link_libraries(executor PRIVATE dl)
#target_include_directories(resource_manager SYSTEM PUBLIC $<TARGET_PROPERTY:PkgConfig::Pistache,INTERFACE_INCLUDE_DIRECTORIES>)
target_link_libraries(resource_manager PRIVATE pistache)

###
# Benchmark apps
###
include(benchmarks)

###
# Examples
###
add_library(functions SHARED examples/functions.cpp)
set_target_properties(functions PROPERTIES POSITION_INDEPENDENT_CODE On)
set_target_properties(functions PROPERTIES LIBRARY_OUTPUT_DIRECTORY examples)
if( ${RFAAS_WITH_EXAMPLES} )
  include(examples)
endif()

###
# Examples
###
if( ${RFAAS_WITH_TESTING} )
  include(testing)
endif()


```

