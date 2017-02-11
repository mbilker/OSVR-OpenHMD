cmake_minimum_required(VERSION 2.8.12)
project(OpenHMDPlugin)

find_package(osvr REQUIRED)

# HIDAPI
set(HIDAPI_DIR "${CMAKE_CURRENT_LIST_DIR}/vendor/hidapi")
include_directories(${HIDAPI_DIR}/hidapi)

IF(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
   	set (HIDAPI_SOURCES ${HIDAPI_DIR}/mac/hid.c)
   	set (LIBS ${LIBS} "-framework CoreFoundation -framework IOKit")
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")

if (WIN32)
   	set (HIDAPI_SOURCES ${HIDAPI_DIR}/windows/hid.c)
   	set (LIBS ${LIBS} setupapi)
endif(WIN32)

IF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
	set (CMAKE_C_STANDARD 99)

   	set (HIDAPI_SOURCES ${HIDAPI_DIR}/libusb/hid.c)
   	find_package(PkgConfig REQUIRED)
   	pkg_search_module(LIBUSB REQUIRED libusb-1.0)
        pkg_search_module(OPENHMD REQUIRED openhmd)
   	set (LIBS ${LIBS} ${LIBUSB_LIBRARIES} ${OPENHMD_LIBRARIES})
   	include_directories(${LIBUSB_INCLUDE_DIRS} ${OPENHMD_INCLUDE_DIRS})
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")

# OSVR Plugin
osvr_convert_json(je_nourish_openhmd_json
    je_nourish_openhmd.json
    "${CMAKE_CURRENT_BINARY_DIR}/je_nourish_openhmd_json.h")

include_directories("${CMAKE_CURRENT_BINARY_DIR}")

osvr_add_plugin(NAME je_nourish_openhmd
    CPP # indicates we'd like to use the C++ wrapper
    SOURCES
    je_nourish_openhmd.cpp
    "${CMAKE_CURRENT_BINARY_DIR}/je_nourish_openhmd_json.h")

target_link_libraries(je_nourish_openhmd ${LIBS} ${OPENHMD_LDFLAGS})
