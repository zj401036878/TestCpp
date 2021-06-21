# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.10.2)

include_directories(include)#引入文件夹


# Declares and names the project.

project("testcppaplication")

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

#编译头文件
#定义全局 my_source_path 变量
file(GLOB my_source_path ${CMAKE_SOURCE_DIR}/*.cpp ${CMAKE_SOURCE_DIR}/*.c)
add_library(tolyCV SHARED ${my_source_path})

#添加动态链接库
add_library(lib_opencv SHARED IMPORTED)
set_target_properties(lib_opencv PROPERTIES IMPORTED_LOCATION ${CMAKE_SOURCE_DIR}/../jniLibs/${ANDROID_ABI}/libopencv_java4.so)


#add_library( # Sets the name of the library.
#             native-lib
#
#             # Sets the library as a shared library.
#             SHARED
#
#             # Provides a relative path to your source file(s).
#             native-lib.cpp )

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

# 在ndk中查找log库 取别名log-lib
find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log)

# 在ndk中查找jnigraphics库 取别名jnigraphics-lib
# jnigraphics包含图形操作的库
find_library(jnigraphics-lib jnigraphics)

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

#设置 target 需要链接的库
target_link_libraries(
        tolyCV
        lib_opencv
        ${jnigraphics-lib}
        ${log-lib})

#target_link_libraries( # Specifies the target library.
#                       native-lib
#
#                       # Links the target library to the log library
#                       # included in the NDK.
#                       ${log-lib} )