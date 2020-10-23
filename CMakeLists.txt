cmake_minimum_required(VERSION 3.10)

project(sqlite3_wrapper)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_library(sqlite3_wrapper INTERFACE)
target_include_directories(sqlite3_wrapper INTERFACE include/)
