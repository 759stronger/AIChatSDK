cmake_minimum_required(VERSION 3.10)

project(LLMTest)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_BUILD_TYPE Debug)

add_executable(LLMTest testLLM.cpp
../chatsdk/src/DeepSeekProvider.cpp
../chatsdk/src/util/my_logger.cpp
../chatsdk/src/ChatGPTProvider.cpp
../chatsdk/src/GeminiProvider.cpp
../chatsdk/src/OllamaDeepSeekProvider.cpp
../chatsdk/src/chat_sdk.cpp
../chatsdk/src/DataManager.cpp
../chatsdk/src/session_manager.cpp
../chatsdk/src/LLMManager.cpp
)

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/build)

include_directories(${PROJECT_SOURCE_DIR}/../chatsdk/include)
include_directories(${PROJECT_SOURCE_DIR}/../chatsdk/include/util)
include_directories(/usr/include/jsoncpp)

find_package(OpenSSL REQUIRED)

include_directories(${OPENSSL_INCLUDE_DIR})

target_compile_definitions(LLMTest PUBLIC
    CPPHTTPLIB_OPENSSL_SUPPORT
)

link_directories(/usr/local/lib)

target_link_libraries(LLMTest fmt jsoncpp OpenSSL::SSL OpenSSL::Crypto spdlog gtest sqlite3)
