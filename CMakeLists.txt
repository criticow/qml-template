cmake_minimum_required(VERSION 3.16)

set(CMAKE_AUTORCC ON)

project(template VERSION 1.0 LANGUAGES CXX)

# To be used but the QML Engine as URI
add_definitions(-DPROJECT_NAME="${PROJECT_NAME}")

find_package(Qt6 6.2 COMPONENTS Quick Gui REQUIRED)

qt_standard_project_setup(REQUIRES 6.5)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

qt_add_executable(${PROJECT_NAME}
  src/main.cpp
)

# qt_add_resources(${PROJECT_NAME} "app_images"
#   PREFIX "/"
#   FILES
#     data/icons/window-minimize-solid.svg
#     data/icons/comment-regular.svg
# )

qt_add_qml_module(${PROJECT_NAME}
  URI ${PROJECT_NAME}
  QML_FILES
    src/Main.qml
)

target_link_libraries(${PROJECT_NAME} PRIVATE Qt6::Gui Qt6::Quick)

include(GNUInstallDirs)
install(TARGETS ${PROJECT_NAME}
  BUNDLE DESTINATION .
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

set(DATA_DESTINATION ${CMAKE_BINARY_DIR}/${CMAKE_BUILD_TYPE}/data)

# postbuild command to copy the data folder
add_custom_command(
  TARGET ${PROJECT_NAME} POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E copy_directory
  ${CMAKE_SOURCE_DIR}/data ${DATA_DESTINATION}
)

add_custom_target(
  copy_data_folder ALL
  DEPENDS ${DATA_DESTINATION}
)