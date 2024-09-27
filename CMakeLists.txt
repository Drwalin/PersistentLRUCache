cmake_minimum_required(VERSION 3.19)
project(persistent_lru_cache)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(CMAKE_COLOR_DIAGNOSTICS ON)

if(CMAKE_BUILD_TYPE STREQUAL "Release")
	set(CMAKE_INTERPROCEDURAL_OPTIMIZATION TRUE)
elseif(CMAKE_BUILD_TYPE STREQUAL "Debug")
else()
endif()

include_directories(include)

aux_source_directory(./include/persistentlrucache/ header_files)
aux_source_directory(./src/ source_files)

add_library(sqlite3 STATIC
	sqlite3.46.1/sqlite3.c
)
add_library(lrucachep STATIC
	${header_files}
	${source_files}
)
target_link_libraries(lrucachep sqlite3)

add_executable(test1 tests/Test1.cpp)
target_link_libraries(test1 lrucachep)
