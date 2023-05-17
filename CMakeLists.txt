cmake_minimum_required(VERSION 3.10)

set(CMAKE_TRY_COMPILE_TRGET_TYPE "static_Library")
project(ExampleProject)

set(CMAKE_CXX_STANDARD 14)

include_directories(src)
ENABLE_TESTING()
add_subdirectory(src)
add_subdirectory(tst)
add_subdirectory(lib/googletest/)


#look for doxygen packet

find_package(Doxygen)

if(DOXYGEN_FOUND)

#set input and output files

    set(DOXYGEN_IN ${CMAKE_CURRENT_SOURCE_DIR}/docs/Doxyfile.in)
    set(DOXYGEN_OUT ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile.out)
   
 #request to configure the file
 
     configure_file(${DOXYGEN_IN} ${DOXYGEN_OUT} @ONLY)
     message("Doxygen build started")  
   
    add_custom_target(docs
        COMMAND ${DOXYGEN_EXECUTABLE} ${DOXYGEN_OUT}
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
        COMMENT "Generating API documentation with Doxygen"
        VERBATIM
    )
else (DOXYGEN_FOUND)

     message("Doxygen need to be installed to generate the doxygen documentation")
     
endif(DOXYGEN_FOUND)
