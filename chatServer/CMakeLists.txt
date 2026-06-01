cmake_minimum_required(VERSION 3.10)

#项目名称
project(AIChatServer)

#设置C++标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

#设置构建类型为debug
set(CMAKE_BUILD_TYPE Debug)

#添加可执行文件
add_executable(AIChatServer chatServer.cpp main.cpp)

#查找openssl包
find_package(OpenSSL REQUIRED)
find_package(Threads REQUIRED)

#包含openssl头文件路径
include_directories(${OPENSSL_INCLUDE_DIR})

#添加编译头文件
target_compile_definitions(AIChatServer PUBLIC CPPHTTPLIB_OPENSSL_SUPPORT)

#设置库的目录
link_directories(/usr/local/lib)

#链接sdk
target_link_libraries(AIChatServer PRIVATE -Wl,--whole-archive ai_chat_sdk -Wl,--no-whole-archive Threads::Threads fmt jsoncpp OpenSSL::SSL   OpenSSL::Crypto
gflags spdlog sqlite3)


