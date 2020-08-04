cmake_minimum_required (VERSION 2.8)

#项目名称
project(TEST)


#编译器参数
set(CMAKE_CXX_COMPILER g++ CACHE STRING "C++ compiler" FORCE)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
add_compile_options(-fPIE -pie -fstack-protector-strong -Wl,-z,relro,-z,now,-z,noexecstack)

# 包匹配
find_package(Threads)
find_package(PkgConfig)
pkg_check_modules(GTKMM gtkmm-3.0)

# 头文件
include_directories(${GTKMM_INCLUDE_DIRS})
include_directories(${PROJECT_SOURCE_DIR})
include_directories(${PROJECT_SOURCE_DIR}/ui)
include_directories(${PROJECT_SOURCE_DIR}/worker)

# 源文件
aux_source_directory(${PROJECT_SOURCE_DIR}/main MAIN_SRCS)
aux_source_directory(${PROJECT_SOURCE_DIR}/ui MAIN_SRCS)
aux_source_directory(${PROJECT_SOURCE_DIR}/worker WORKER_SRCS)


# 指定库文件连接目录
link_directories(
    ${GTKMM_LIBRARY_DIRS}
    ${CMAKE_BINARY_DIR}
)

# 项目生成
add_executable(test_gui_multithread
    ${MAIN_SRCS}
    ${MAIN_SRCS}
    ${WORKER_SRCS}
)
target_link_libraries(test_gui_multithread
    ${CMAKE_THREAD_LIBS_INIT}
    ${GTKMM_LIBRARIES} 
)
