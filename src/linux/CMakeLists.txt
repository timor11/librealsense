# License: Apache 2.0. See LICENSE file in root directory.
# Copyright(c) 2019 Intel Corporation. All Rights Reserved.

# My new line
target_sources(${LRS_TARGET}
    PRIVATE
        "${CMAKE_CURRENT_LIST_DIR}/backend-v4l2.cpp"
        "${CMAKE_CURRENT_LIST_DIR}/backend-hid.cpp"
        "${CMAKE_CURRENT_LIST_DIR}/backend-v4l2.h"
        "${CMAKE_CURRENT_LIST_DIR}/backend-hid.h"
)

include(libusb_config)
target_link_libraries(${LRS_TARGET} PRIVATE usb)
if(USE_EXTERNAL_USB)
    add_dependencies(${LRS_TARGET} libusb)
endif()

include(${CMAKE_CURRENT_LIST_DIR}/find_udev.cmake)
if(UDEV_FOUND)
    target_sources(${LRS_TARGET}
        PRIVATE
            "${CMAKE_CURRENT_LIST_DIR}/udev-device-watcher.cpp"
            "${CMAKE_CURRENT_LIST_DIR}/udev-device-watcher.h"
    )
    target_link_libraries(${LRS_TARGET} PRIVATE udev)
    add_definitions(-DUSING_UDEV)
else()
    message(STATUS "UDEV not found; using polling device-watcher!")
endif()
