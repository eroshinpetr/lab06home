cmake_minimum_required(VERSION 3.10)

project(solver VERSION 0.1.0.0)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(formatter STATIC
  formatter_lib/formatter.cpp
)

target_include_directories(formatter
  PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib
)

add_library(formatter_ex STATIC
  formatter_ex_lib/formatter_ex.cpp
)

target_include_directories(formatter_ex
  PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib
    ${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib
)

target_link_libraries(formatter_ex
  PUBLIC
    formatter
)

add_library(solver_lib STATIC
  solver_lib/solver.cpp
)

target_include_directories(solver_lib
  PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/solver_lib
)

add_executable(solver
  solver_application/equation.cpp
)

target_include_directories(solver
  PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib
    ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib
    ${CMAKE_CURRENT_SOURCE_DIR}/solver_lib
)

target_link_libraries(solver
  PRIVATE
    formatter_ex
    solver_lib
)

install(TARGETS solver
  RUNTIME DESTINATION bin
  BUNDLE DESTINATION .
)

include(CPackConfig.cmake)
