﻿cmake_minimum_required (VERSION 3.4)
set(CMAKE_OSX_DEPLOYMENT_TARGET "10.9" CACHE STRING "Minimum OS X deployment version")
project (QR-Code-Generator)

include(conanbuildinfo.cmake)
conan_basic_setup()

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if(MSVC)
    add_definitions (-D_WIN32_WINNT=0x0601)
    add_definitions (-D_SCL_SECURE_NO_WARNINGS=1)
    add_definitions (-D_CRT_SECURE_NO_WARNINGS=1)

    set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS TRUE)
    set(CMAKE_VS_INCLUDE_INSTALL_TO_DEFAULT_BUILD 1)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /EHsc /utf-8")
endif()

if(WIN32 AND BUILD_SHARED_LIBS)
    set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS TRUE)
    set(CMAKE_VS_INCLUDE_INSTALL_TO_DEFAULT_BUILD TRUE)
endif()

set(SRC source_subfolder/cpp/QrCode.cpp
        source_subfolder/cpp/QrSegment.cpp
        source_subfolder/cpp/BitBuffer.cpp
        source_subfolder/cpp/QrCodeGeneratorWorker.cpp)

add_library(qrcodegen ${SRC})

INSTALL(TARGETS qrcodegen
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib)

set(HEADERS source_subfolder/cpp/QrCode.hpp
            source_subfolder/cpp/QrSegment.hpp
            source_subfolder/cpp/BitBuffer.hpp)

INSTALL(FILES ${HEADERS} DESTINATION include/qrcodegen)
