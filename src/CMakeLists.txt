cmake_minimum_required(VERSION 3.5)

project(3DViewer_2 VERSION 2.0 LANGUAGES CXX C) 

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# only for school macs
# set(ENV{QT_PLUGIN_PATH} "/usr/local/Qt-6.6.2/plugins/")
# set(ENV{CMAKE_PREFIX_PATH} "/usr/local/Qt-6.6.2/") 

find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets)
find_package(Qt6 REQUIRED COMPONENTS OpenGLWidgets)
find_package(Qt6 REQUIRED COMPONENTS Gui)
add_subdirectory(view/QtGifImage)


set(PROJECT_SOURCES
        main.cc
        view/view.cc
        view/view.h
        view/mainwindow.ui
        view/myopenglwidget.cc
        view/myopenglwidget.h
        model/object.cc
        model/object.h
        model/model.cc
        model/model.h
        model/parser.cc
        model/parser.h
        model/affinity.cc
        model/affinity.h
        controller/controller.cc
        controller/controller.h
)

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(3DViewer_2
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
    )
# Define target properties for Android with Qt 6 as:
#    set_property(TARGET 3DViewer_2 APPEND PROPERTY QT_ANDROID_PACKAGE_SOURCE_DIR
#                 ${CMAKE_CURRENT_SOURCE_DIR}/android)
# For more information, see https://doc.qt.io/qt-6/qt-add-executable.html#target-creation
else()
    if(ANDROID)
        add_library(3DViewer_2 SHARED
            ${PROJECT_SOURCES}
        )
# Define properties for Android with Qt 5 after find_package() calls as:
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
    else()
        add_executable(3DViewer_2
            ${PROJECT_SOURCES}
        )
    endif()
endif()

target_link_libraries(3DViewer_2 PRIVATE Qt${QT_VERSION_MAJOR}::Widgets)
target_link_libraries(3DViewer_2 PRIVATE Qt6::OpenGLWidgets)
target_link_libraries(3DViewer_2 PRIVATE Qt6::Gui)
target_link_libraries(3DViewer_2 PRIVATE GIFQT)


set_target_properties(3DViewer_2 PROPERTIES
    MACOSX_BUNDLE_GUI_IDENTIFIER my.example.com
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

if(QT_VERSION_MAJOR EQUAL 6)
    qt_finalize_executable(3DViewer_2)
endif()
