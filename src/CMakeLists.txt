cmake_minimum_required(VERSION 3.10)
project(BigDeal)

# Set the C standard to C99
set(CMAKE_C_STANDARD 99)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Compiler options
add_compile_options(-O -Wall -pedantic)

# Include directory for header files
include_directories(${PROJECT_SOURCE_DIR}/include)

# Specify the source files
set(COMMON_SOURCES
    src/rmd160.c
    src/mp.c
    src/binomial.c
)

set(SAFEOBJS_SOURCES
    src/main.c
    src/collect.c
    src/output.c
)

set(EXTNOBJS_SOURCES
    src/main.c
    src/collect.c
    src/output.c
)

# Create executable for bigdeal
add_executable(bigdeal ${COMMON_SOURCES} ${SAFEOBJS_SOURCES} src/unix.c)
target_compile_definitions(bigdeal PRIVATE)

# Create executable for bigdealx
add_executable(bigdealx ${COMMON_SOURCES} ${EXTNOBJS_SOURCES} src/unix.c)
target_compile_definitions(bigdealx PRIVATE BIGDEALX)

# Define OS-specific source file
if(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
    set(OS_SRC src/dos.c)
elseif(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    set(OS_SRC src/unix.c)
endif()

# Clean script
file(WRITE ${CMAKE_BINARY_DIR}/clean.cmake
"file(REMOVE
    ${CMAKE_BINARY_DIR}/${COMMON_SOURCES}
    ${CMAKE_BINARY_DIR}/${SAFEOBJS_SOURCES}
    ${CMAKE_BINARY_DIR}/${EXTNOBJS_SOURCES}
    ${CMAKE_BINARY_DIR}/${OS_SRC}
)")

