# cmake_minimum_required(VERSION 3.10)

# project(TaskQueue)

# # 设置C++标准
# set(CMAKE_CXX_STANDARD 17)
# set(CMAKE_CXX_STANDARD_REQUIRED ON)
# set(CMAKE_CXX_EXTENSIONS OFF)

# aux_source_directory(. SRC_CUR)

# add_executable(main ${SRC_CUR})

# # 查找线程库
# find_package(Threads REQUIRED)

# # 链接线程库到可执行文件
# target_link_libraries(main PRIVATE Threads::Threads)

cmake_minimum_required(VERSION 3.10)

# 项目名称和版本
project(TaskQueue VERSION 1.0)

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# 设置编译选项
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug")
endif()

# Debug模式下的编译选项
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g -O0 -Wall -Wextra -Wpedantic")
# Release模式下的编译选项
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O3")

# 源文件列表
set(SOURCES
    event.cpp
    example.cpp
    task_queue.cpp
    task_queue_base.cpp
    task_queue_manager.cpp
    task_queue_std.cpp
)

# 头文件列表
set(HEADERS
    event.h
    queued_task.h
    task_queue.h
    task_queue_base.h
    task_queue_manager.h
    task_queue_std.h
)

# 添加可执行文件
add_executable(${PROJECT_NAME} ${SOURCES} ${HEADERS})

# 链接 pthread 库
find_package(Threads REQUIRED)
target_link_libraries(${PROJECT_NAME} PRIVATE Threads::Threads)

# 设置输出目录
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

# Debug 相关配置
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    # 启用调试信息
    target_compile_definitions(${PROJECT_NAME} PRIVATE DEBUG_MODE)
    # 添加调试符号
    target_compile_options(${PROJECT_NAME} PRIVATE -g)
    # 禁用优化
    target_compile_options(${PROJECT_NAME} PRIVATE -O0)
    # 启用所有警告
    target_compile_options(${PROJECT_NAME} PRIVATE 
        -Wall 
        -Wextra 
        -Wpedantic 
        -Wconversion 
        -Wshadow 
        -Wcast-align
        -Wunused
        -Woverloaded-virtual
        -Wcast-qual
        -Wold-style-cast
    )
    # 将警告视为错误
    # target_compile_options(${PROJECT_NAME} PRIVATE -Werror)
endif()

# 添加安装规则
install(TARGETS ${PROJECT_NAME}
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)

# 安装头文件
install(FILES ${HEADERS} DESTINATION include/${PROJECT_NAME})

# 打印构建类型信息
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
message(STATUS "C++ compiler: ${CMAKE_CXX_COMPILER}")
message(STATUS "C++ flags: ${CMAKE_CXX_FLAGS}")
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    message(STATUS "Debug flags: ${CMAKE_CXX_FLAGS_DEBUG}")
else()
    message(STATUS "Release flags: ${CMAKE_CXX_FLAGS_RELEASE}") 
endif()

