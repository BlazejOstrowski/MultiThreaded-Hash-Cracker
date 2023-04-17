 #Set the minimum version of CMake that can be used
 # To find the cmake version run
 # $ cmake --version
 cmake_minimum_required(VERSION 3.5)

 # Set the project name
 project (hash_cracker)

 # Create a sources variable with a link to all cpp files to compile
 set(SOURCES
         src/Cracker.cpp
         src/HashAlgorithm.cpp
         src/MD5Algorithm.cpp
         src/pch.cpp
         src/SHA1Algorithm.cpp
         src/SHA256Algorithm.cpp
         src/main.cpp
         )

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread")

 # Add an executable with the above sources
 add_executable(hash_cracker ${SOURCES})

 # Set the directories that should be included in the build command for this target
 # when running g++ these will be included as -I/directory/path/
 target_include_directories(hash_cracker
         PRIVATE
         ${PROJECT_SOURCE_DIR}/inc
         )

 find_package(OpenSSL REQUIRED)
 if (OPENSSL_FOUND)
  # Add the include directories for compiling
  target_include_directories(${PROJECT_NAME} PUBLIC ${OPENSSL_INCLUDE_DIR})

  # Add the static lib for linking
  target_link_libraries(${PROJECT_NAME} OpenSSL::SSL OpenSSL::Crypto)

  message(STATUS "Found OpenSSL ${OPENSSL_VERSION}")

 else()

  message(STATUS "OpenSSL Not Found")

 endif()