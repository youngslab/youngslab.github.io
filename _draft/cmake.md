

# CMake




# Snippet

```
CMAKE_MINIMUM_REQUIRED (VERSION 2.6)

# project configurations.
PROJECT (TEST)

# flags.
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++1y")

# add sources to be built.
set(SRC main.cpp)

# set a target.
add_executable(test ${SRC})

# libraries.
# target_link_libraries(test lib)
```
