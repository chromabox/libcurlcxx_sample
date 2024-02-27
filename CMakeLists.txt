cmake_minimum_required(VERSION 3.22)

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(DEFAULT_BUILD_TYPE "Release")
set(CMAKE_C_FLAGS_DEBUG "-g3 -Og")
set(CMAKE_C_FLAGS_RELEASE "-g -O2")


project(sample)

add_subdirectory(external_libs/libcurlcxx)


set(INC_DIRS
  include
)

set(SRC_FILES
src/sample.cpp
)


add_executable(${PROJECT_NAME} WIN32 ${SRC_FILES})

target_include_directories(${PROJECT_NAME} PUBLIC ${INC_DIRS} ${LIBCURLCXX_INC_DIRS})
target_compile_features(${PROJECT_NAME} PUBLIC cxx_std_20)

target_link_libraries(${PROJECT_NAME} PRIVATE curlcxx ${CMAKE_THREAD_LIBS_INIT})
