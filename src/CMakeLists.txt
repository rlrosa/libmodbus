# Enable C++11
#
# NOTE: Won't work when cross-compiling; need to explicitly add -std=c++11 in
# toolchain file for that.
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(modbus_NAME modbus)

set(LIBMODBUS_VERSION_MAJOR 3)
set(LIBMODBUS_VERSION_MINOR 1)
set(LIBMODBUS_VERSION_MICRO 6)
set(LIBMODBUS_VERSION "3.1.6")
configure_file("modbus-version.h.in" "modbus-version.h")

set(modbus_SOURCES
    modbus.c
    modbus-rtu.c
    modbus-data.c
    modbus-tcp.c
)

set(modbus_INCLUDES
    modbus.h
    modbus-rtu.h
    modbus-tcp.h
    modbus-version.h
)

add_library(${modbus_NAME} SHARED ${modbus_SOURCES})
target_include_directories(${modbus_NAME} PRIVATE ./)

if(WIN32)
    target_link_libraries(${modbus_NAME} ws2_32)
endif(WIN32)

install(TARGETS ${modbus_NAME} DESTINATION lib)
install(FILES ${modbus_INCLUDES} DESTINATION include/${modbus_NAME})
