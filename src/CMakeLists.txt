set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

set(SDK_PATH "idasdk68" CACHE FILEPATH "IDA SDK path")
set(LIB_PATH "idasdk68" CACHE FILEPATH "IDA libraries path")

include_directories(${SDK_PATH}/include)
link_directories(${SDK_PATH} ${SDK_PATH}/bin ${LIB_PATH})

add_definitions(-D__IDP__ -D__PLUGIN__)
if(WIN32)
    add_definitions(-D__NT__)
endif()
if(UNIX AND NOT APPLE)
    add_definitions(-D__LINUX__)
endif()
if(APPLE)
    add_definitions(-D__MAC__)
endif()

add_library(teloader SHARED teloader.cpp)
# binary is always i386
set_target_properties(teloader PROPERTIES COMPILE_FLAGS "-m32" LINK_FLAGS "-m32")
#set_target_properties(teloader PROPERTIES SUFFIX ${EXT})
target_link_libraries(teloader ida)

add_library(teloader64 SHARED teloader.cpp)
# binary is always i386
set_target_properties(teloader64 PROPERTIES COMPILE_FLAGS "-m32" LINK_FLAGS "-m32")
target_compile_definitions(teloader64 PUBLIC -D__EA64__)
#set_target_properties(teloader64 PROPERTIES SUFFIX ${EXT64})
target_link_libraries(teloader64 ida64)
