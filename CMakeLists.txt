cmake_minimum_required (VERSION 3.9)

project (acpica C CXX)

file(GLOB_RECURSE SOURCE_ALL 
    ./source/components/dispatcher/*.c
    ./source/components/events/*.c
    ./source/components/namespace/*.c
    ./source/components/parser/*.c
    ./source/components/executer/*.c
    ./source/components/hardware/*.c
    ./source/components/tables/*.c
    ./source/components/utilities/*.c
)
set_source_files_properties(${SOURCE_ALL} PROPERTIES COMPILE_FLAGS 
    "${CXX_X64} ${FREE_C_FLAGS} ${KERNEL_CXX_FLAGS}")

add_library(acpica STATIC ${SOURCE_ALL})
target_include_directories(acpica PUBLIC source/include)
set_target_properties(acpica PROPERTIES LINK_FLAGS -s)
add_custom_command(TARGET acpica POST_BUILD COMMAND
    objcopy --localize-symbols ${CMAKE_CURRENT_SOURCE_DIR}/hidden-symbols.txt ${CMAKE_ARCHIVE_OUTPUT_DIRECTORY}/libacpica.a)

