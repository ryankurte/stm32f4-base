# Base STM32F4 CMake file
#
# This can be used as is as a project base, or by adding the stm32f4-base
# repository as a submodule to another project, copying this CMakeLists file
# to the top level directory, and updating the BASE_LOCATION variable to reflect this
# change

###### Project Environment #####

# Set minimum CMake version
cmake_minimum_required(VERSION 2.8)

# Set the compiler (must be prior to project setup)
include(../../toolchain/arm-gcc.cmake)

##### Project Setup #####

# Set our output target
set(TARGET stm32-test)

# Configure project and languages
project(${TARGET} C CXX ASM)

# Set device
set(DEVICE STM32F746NGH6U)
set(RAM_LENGTH 340k)

# Set BSP (Evaluation kits only)
set(USE_HAL ON)
set(BSP "STM32F746G-Discovery")
set(BSP_COMPONENTS "Common")
set(FATFS_DRIVER sd_diskio)
add_definitions(-DUSE_STM32746G_DISCO=1)

set(USE_STLINK ON)

# Set build
if(NOT CMAKE_BUILD_TYPE)
set(CMAKE_BUILD_TYPE DEBUG)
endif()

##### Modules #####

# Libraries can be added to the LIBS variable
# or manually included here.

# Add base libs (emlib, CMSIS, device files)
include(${PROJECT_SOURCE_DIR}/../../toolchain/stm32-base.cmake)

##### Files #####

# Add project files
include(${PROJECT_SOURCE_DIR}/example.cmake)

##### Outputs #####

# Generate executable and link
add_executable(${TARGET}.elf ${PROJECT_SOURCES})
target_link_libraries(${TARGET}.elf ${LIBS})

##### Post build (require target to be defined) #####

# Add post build commands
include(${PROJECT_SOURCE_DIR}/../../toolchain/post-build.cmake)

# Add JLink commands
if(USE_STLINK)
include(${PROJECT_SOURCE_DIR}/../../toolchain/stlink.cmake)
endif()

if(USE_JLINK)
include(${PROJECT_SOURCE_DIR}/../../toolchain/stlink.cmake)
endif()
