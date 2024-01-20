# CMakeList.txt : CMake project for v8dasm-msvc, include source and define
# project specific logic here.
#
cmake_minimum_required (VERSION 3.8)

# Enable Hot Reload for MSVC compilers if supported.
if (POLICY CMP0141)
  cmake_policy(SET CMP0141 NEW)
  set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif()

project ("v8dasm-msvc")

set(CompilerFlags
        CMAKE_CXX_FLAGS
        CMAKE_CXX_FLAGS_DEBUG
        CMAKE_CXX_FLAGS_RELEASE
        CMAKE_C_FLAGS
        CMAKE_C_FLAGS_DEBUG
        CMAKE_C_FLAGS_RELEASE
        )
foreach(CompilerFlag ${CompilerFlags})
  string(REPLACE "/MD" "/MT" ${CompilerFlag} "${${CompilerFlag}}")
endforeach()

set(CMAKE_CXX_STANDARD 17)

include_directories("D:\\github\\v8-disa\\nodejs\\deps\\v8\\include")

link_directories("D:\\github\\v8-disa\\nodejs\\out\\Release\\lib")

link_libraries(v8_snapshot v8_compiler v8_libbase v8_libplatform v8_base_without_compiler v8_libsampler  icutools v8_zlib)

# Add source to this project's executable.
add_executable (v8dasm-msvc "v8dasm-msvc.cpp" "v8dasm-msvc.h")

if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET v8dasm-msvc PROPERTY CXX_STANDARD 20)
endif()

