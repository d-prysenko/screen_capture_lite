cmake_minimum_required(VERSION 3.16)
project(screen_capture_lite_build VERSION 17.1)

set(CMAKE_CXX_STANDARD 17)
#set(CMAKE_DEBUG_POSTFIX "d")
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF) 
option(BUILD_EXAMPLE "Build example" ON)
option(BUILD_CSHARP "Build C#" ON)
option(BUILD_SHARED_LIBS "Build shared libraries" ON)
set (CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
set (CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})

if (NOT EXISTS ${CMAKE_BINARY_DIR}/CMakeCache.txt)
  if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "" FORCE)
  endif()
endif()

if(MSVC)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4 -D_SILENCE_CXX20_OLD_SHARED_PTR_ATOMIC_SUPPORT_DEPRECATION_WARNING")
else()
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -pedantic")
endif()


set(TARGET_SUFFIX static)


set(TARGET_NAME screen_capture_lite_static)

add_subdirectory(src_cpp) 


install(
  TARGETS ${TARGET_NAME}
  EXPORT screen_capture_lite
  ARCHIVE  DESTINATION ${CMAKE_INSTALL_LIBDIR}
  LIBRARY  DESTINATION ${CMAKE_INSTALL_LIBDIR}
  PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
  RUNTIME  DESTINATION ${CMAKE_INSTALL_BINDIR}
)

#configure_file(modules/${TARGET_NAME}Config.cmake.in ${TARGET_NAME}Config.cmake @ONLY)

#export(TARGETS ${TARGET_NAME} FILE ${TARGET_NAME}-config.cmake)
#install(EXPORT ${TARGET_NAME}-config FILE ${TARGET_NAME}-config.cmake DESTINATION lib/cmake/screen_capture_lite)

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${TARGET_NAME}Config.cmake
  DESTINATION lib${LIB_SUFFIX}/cmake/screen_capture_lite)

 
install(FILES 
	include/ScreenCapture.h 
	DESTINATION include
)

install(
  EXPORT screen_capture_lite
  DESTINATION lib${LIB_SUFFIX}/cmake/screen_capture_lite
  FILE ${TARGET_NAME}Config.cmake
)

#install(EXPORT screen_capture_lite_build
#    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/screen_capture_lite_build
#    FILE        screen_capture_lite_buildConfig.cmake)
