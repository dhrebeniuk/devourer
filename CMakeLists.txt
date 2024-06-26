cmake_minimum_required(VERSION 3.15)

project(Rtl8812auNet)

# the `pkg_check_modules` function is created with this call
find_package(PkgConfig REQUIRED)

# this calls create special `PkgConfig::LIBUSB` variables
pkg_check_modules(LIBUSB REQUIRED IMPORTED_TARGET libusb-1.0)

find_package(spdlog CONFIG REQUIRED)

add_executable(WiFiDriver
    src/logger.h
    src/main.cpp

    hal/Hal8812PhyReg.h
    hal/Hal8812PwrSeq.c
    hal/Hal8812PwrSeq.h
    hal/basic_types.h
    hal/hal8812a_fw.c
    hal/hal8812a_fw.h
    hal/hal_com_reg.h
    hal/rtl8812a_hal.h
    hal/rtl8812a_recv.h
    hal/rtl8812a_spec.h

    src/EepromManager.cpp
    src/EepromManager.h
    src/Firmware.h
    src/FirmwareManager.cpp
    src/FirmwareManager.h
    src/FrameParser.cpp
    src/FrameParser.h
    src/HalModule.cpp
    src/HalModule.h
    src/ParsedRadioPacket.cpp
    src/RadioManagementModule.cpp
    src/RadioManagementModule.h
    src/Rtl8812aDevice.cpp
    src/Rtl8812aDevice.h
    src/RtlUsbAdapter.cpp
    src/RtlUsbAdapter.h
    src/SelectedChannel.h
    src/WiFiDriver.cpp
    src/WiFiDriver.h
    src/registry_priv.h
    )

target_link_libraries(WiFiDriver PRIVATE
    PkgConfig::LIBUSB
    # use the header-only version
    spdlog::spdlog_header_only
    )

set_property(TARGET WiFiDriver PROPERTY CXX_STANDARD 20)
target_include_directories(WiFiDriver PRIVATE hal)

target_compile_options(WiFiDriver PRIVATE -fno-omit-frame-pointer -fsanitize=address)
target_link_options(WiFiDriver PRIVATE -fsanitize=address)
