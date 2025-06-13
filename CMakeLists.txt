cmake_minimum_required(VERSION 3.10)
project(math VERSION 1.0 LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_library(math src/math.cpp)
add_library(math::math ALIAS math)
target_include_directories(math PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
$<INSTALL_INTERFACE:include>
)

# Install rules
install(TARGETS math
        EXPORT mathTarget
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)

install(DIRECTORY include/ DESTINATION include)

# Export the target
install(EXPORT mathTarget
        FILE mathTarget.cmake
        NAMESPACE math::
        DESTINATION lib/cmake/math)

# Generate a config file
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/mathConfigVersion.cmake"
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY SameMajorVersion
)

install(FILES
  "${CMAKE_CURRENT_SOURCE_DIR}/cmake/mathConfig.cmake"
  "${CMAKE_CURRENT_BINARY_DIR}/mathConfigVersion.cmake"
  DESTINATION lib/cmake/math)