cmake_minimum_required(VERSION 3.1)

project(v8pp)

set(EXEC v8pp_test)

set(CMAKE_CXX_STANDARD 14)

set(CMAKE_CXX_FLAGS "-Wall -Wextra -std=c++11 -fPIC -fno-rtti -DV8PP_ISOLATE_DATA_SLOT=0")


include_directories(~/nodejs/node-v14.0.0/deps/v8/include)
include_directories(./)
include_directories(./v8pp)
include_directories(./test)


link_directories(~/nodejs/node-v14.0.0/out/Release)


file(GLOB SOURCES test/*.cpp)

file(GLOB LIBSOURCES v8pp/*.cpp)

add_library(v8pp STATIC ${LIBSOURCES})
#target_include_directories(v8pp STATIC ./)


add_library(file SHARED plugins/file.cpp)
target_link_libraries(file v8_snapshot v8_libplatform v8_base_without_compiler v8_libplatform v8_libbase v8_libsampler v8_zlib v8_compiler v8_initializers v8pp dl pthread)


add_library(console SHARED plugins/console.cpp)
target_link_libraries(console v8_snapshot v8_libplatform v8_base_without_compiler v8_libplatform v8_libbase v8_libsampler v8_zlib v8_compiler v8_initializers v8pp dl pthread)



add_executable(${EXEC} ${SOURCES})
target_link_libraries(${EXEC} v8_snapshot v8_libplatform v8_base_without_compiler v8_libplatform v8_libbase v8_libsampler v8_zlib v8_compiler v8_initializers v8pp dl pthread)

