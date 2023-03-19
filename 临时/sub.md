when i use archlinux to build the project,I had some problems building files with cmake. I've solved it, but I guess the problem is general.(at least in archlinux).

After executing `cmake -DCMAKE_CXX_COMPILER=clang -DCMAKE_BUILD_TYPE=Release .`, the error is reported as follows：

```bash
-- Configuring done
CMake Error at CMakeLists.txt:113 (add_dependencies):
  The dependency target "cereal" of target "rdmalib" does not exist.


CMake Error at CMakeLists.txt:132 (add_dependencies):
  The dependency target "cereal" of target "rfaaslib" does not exist.


CMake Error at CMakeLists.txt:117 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error:
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error at CMakeLists.txt:135 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error at CMakeLists.txt:136 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error:
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error:
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error at CMakeLists.txt:180 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error at CMakeLists.txt:180 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error:
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error:
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error at CMakeLists.txt:180 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error at CMakeLists.txt:180 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error:
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error:
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error at CMakeLists.txt:180 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error at CMakeLists.txt:180 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error:
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error:
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.


CMake Error at cmake/benchmarks.cmake:3 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.
Call Stack (most recent call first):
  CMakeLists.txt:196 (include)


CMake Error at cmake/benchmarks.cmake:3 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.
Call Stack (most recent call first):
  CMakeLists.txt:196 (include)


CMake Error at cmake/benchmarks.cmake:17 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.
Call Stack (most recent call first):
  CMakeLists.txt:196 (include)


CMake Error at cmake/benchmarks.cmake:18 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.
Call Stack (most recent call first):
  CMakeLists.txt:196 (include)


CMake Error at cmake/benchmarks.cmake:17 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.
Call Stack (most recent call first):
  CMakeLists.txt:196 (include)


CMake Error at cmake/benchmarks.cmake:18 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.
Call Stack (most recent call first):
  CMakeLists.txt:196 (include)


CMake Error at cmake/benchmarks.cmake:17 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.
Call Stack (most recent call first):
  CMakeLists.txt:196 (include)


CMake Error at cmake/benchmarks.cmake:18 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.
Call Stack (most recent call first):
  CMakeLists.txt:196 (include)


CMake Error at cmake/benchmarks.cmake:17 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.
Call Stack (most recent call first):
  CMakeLists.txt:196 (include)


CMake Error at cmake/benchmarks.cmake:18 (target_include_directories):
  Error evaluating generator expression:

    $<TARGET_PROPERTY:cereal,INTERFACE_INCLUDE_DIRECTORIES>

  Target "cereal" not found.
Call Stack (most recent call first):
  CMakeLists.txt:196 (include)


-- Generating done
CMake Generate step failed.  Build files cannot be regenerated correctly.
```

I found a similar problem in google: https://github.com/conan-io/conan-center-index/issues/2151, after understanding, I found that one of the solutions is to put the CMakeLIsts.txt file The `cereal` in lines 113, 117, 132, and 136 is changed to `cereal::cereal`, but there may be a better solution, what do you think?

I'm not quite sure what the question means, but I guess it's at least helpful to the project.