cmake_minimum_required(VERSION 3.19)
project(Diplom LANGUAGES CXX)

find_package(Qt6 6.5 REQUIRED COMPONENTS Core Widgets)
find_package(OpenCV REQUIRED)
find_package(OpenSSL REQUIRED)
find_package(dlib REQUIRED)
find_package(Eigen3 REQUIRED)

qt_standard_project_setup()

qt_add_executable(Diplom
    WIN32 MACOSX_BUNDLE
    main.cpp
    mainwindow.cpp
    mainwindow.h
    mainwindow.ui
    database.h
    database.cpp
    .gitignore
    .env
)

target_link_libraries(Diplom
    PRIVATE
        Qt6::Core
        Qt6::Widgets
        OpenSSL::SSL
        OpenSSL::Crypto
        ${OpenCV_LIBS}
        dlib::dlib
        Eigen3::Eigen
)

target_include_directories(${PROJECT_NAME} PRIVATE
    ${OpenCV_INCLUDE_DIRS}
    ${OPENSSL_INCLUDE_DIR}
)

include(GNUInstallDirs)

install(TARGETS Diplom
    BUNDLE  DESTINATION .
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

qt_generate_deploy_app_script(
    TARGET Diplom
    OUTPUT_SCRIPT deploy_script
    NO_UNSUPPORTED_PLATFORM_ERROR
)
install(SCRIPT ${deploy_script})

