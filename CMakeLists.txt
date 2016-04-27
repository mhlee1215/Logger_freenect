cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

project(kinect_capture)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")

find_package(libfreenect REQUIRED)
find_package(PkgConfig REQUIRED)


#set(FREENECT_DIR)
#set(FREENECT_BUILD_DIR)
#set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${FREENECT_DIR}/cmake_modules/")


#Boost
#set(BOOST_ROOT)
find_package(Boost REQUIRED)
set(BOOST_LIBRARYDIR ${Boost_LIBRARIES})
set(Boost_USE_STATIC_LIBS        ON)
set(Boost_USE_MULTITHREADED      ON)
set(Boost_USE_STATIC_RUNTIME    OFF)
find_package(Boost COMPONENTS date_time)
include_directories(${Boost_INCLUDE_DIRS})
link_directories(${BOOST_LIBRARYDIR})



find_package(Boost COMPONENTS filesystem)
set(Boost_filesystem_lib ${Boost_LIBRARIES})

find_package(Boost COMPONENTS system)
set(Boost_system_lib ${Boost_LIBRARIES})
#VTK
#find_package(VTK)
#if(VTK_FOUND)
#   INCLUDE(${VTK_USE_FILE})
#endif() f

#PCL
#find_package(PCL 1.1 COMPONENTS common visualization)
#include_directories(${PCL_INCLUDE_DIRS})
#link_directories(${PCL_LIBRARY_DIRS})
#add_definitions(${PCL_DEFINITIONS})

#OpenCV
#set(OpenCV_DIR)
#find_package(OpenCV COMPONENTS filesystem)
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})
link_directories(${OpenCV_LIB_DIR})

#OpenGL
find_package(OpenGL REQUIRED)
find_package(GLUT REQUIRED)
include_directories(${OPENGL_INCLUDE_DIRS} ${GLUT_INCLUDE_DIRS} ${USB_INCLUDE_DIRS})
#include_directories("C:/code/glew-1.7.0/include")
#include_directories("C:/code/glew-1.7.0/lib")

#Glui
#find_package(libglui2c2 REQUIRED)
#pkg_search_module(GLUI REQUIRED "/home/mhlee/LIBS/glui-2.36/src")
#set(GLUI_INCLUDE "/home/mhlee/LIBS/glui-2.36/src/include/")
#set(GLUI_LIBRARIES "/home/mhlee/LIBS/glui-2.36/src/lib/libglui.a")
set(GLUI_INCLUDE "/usr/include/GL/")
set(GLUI_LIBRARIES "/usr/lib/libglui.a")
include_directories(${GLUI_INCLUDE})
link_directories(${GLUI_LIBRARIES})

#PTHREAD
#set(THREADS_PTHREADS_INCLUDE_DIR)
#set(THREADS_PTHREADS_WIN32_LIBRARY)
#set(THREADS_USE_PTHREADS_WIN32 true)
find_package(Threads REQUIRED)
include_directories(${THREADS_PTHREADS_INCLUDE_DIR})

#USB
#set(LIBUSB_1_INCLUDE_DIR)
#set(LIBUSB_1_LIBRARY)
#find_package(libusb-1.0 REQUIRED)
#include_directories(${LIBUSB_1_INCLUDE_DIRS})
find_package(PkgConfig REQUIRED)
pkg_search_module(LIBUSB1 REQUIRED libusb-1.0)
include_directories(SYSTEM ${LIBUSB1_INCLUDE_DIRS})
#include_directories("${FREENECT_DIR}/platform/windows")
#include_directories("${FREENECT_DIR}/platform/windows/libusb10emu")

#Freenect
#include_directories("${FREENECT_DIR}/include")
#set(FREENECT_LIBRARY_DIR optimized "${FREENECT_BUILD_DIR}/lib/Release/" debug "${FREENECT_BUILD_DIR}/lib/Debug/")
#set(FREENECT_LIBRARY_DIR "${FREENECT_BUILD_DIR}/lib/")
#link_directories(${FREENECT_LIBRARY_DIR})
#set(FREENECT_LIBRARIES "freenect")

#Files
file(GLOB_RECURSE INCS "*.h")
set(SRCS 
    kinect_capture.cpp
	kinect_capture_app.cpp 
	kinect_calibration.cpp 
	freenect_module.cpp
)

#MESSAGE(${OpenCV_LIB_DIR})

#Build
add_executable (kinect_capture ${SRCS} ${INCS})
target_link_libraries (kinect_capture 
	${FREENECT_LIBRARIES} 
	${CMAKE_THREAD_LIBS_INIT} 
	${OPENGL_LIBRARIES} 
	${GLUT_LIBRARIES} 
	${GLUI_LIBRARIES}
	${LIBUSB_1_LIBRARIES} 
	${OpenCV_LIBS} 
	${BOOST_LIBRARYDIR}
	${Boost_filesystem_lib}
	${Boost_system_lib}
	)
#${PCL_LIBRARIES} ${VTK_LIBRARIES}
