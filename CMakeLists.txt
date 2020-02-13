# CMAKE version of Ubuntu 16.04 is 3.5.
cmake_minimum_required(VERSION 3.5)
project(pj_name)

set(LINKLIBS)
set(COMPILE_OPTION)
set(INCL)
set(SRC)

################ Qt5 FrameWork #################

if(UNIX AND NOT APPLE)
  list(APPEND LINKLIBS
    ${PROJECT_SOURCE_DIR}/lib/libQt5Widgets.so.5
    ${PROJECT_SOURCE_DIR}/lib/libQt5Core.so.5
    ${PROJECT_SOURCE_DIR}/lib/libQt5Gui.so.5

    ### X11 ###
    ${PROJECT_SOURCE_DIR}/lib/libQt5XcbQpa.so.5
    ${PROJECT_SOURCE_DIR}/lib/platforms/libqxcb.so

    ### Input ###
    ${PROJECT_SOURCE_DIR}/lib/platforminputcontexts/libcomposeplatforminputcontextplugin.so
    ${PROJECT_SOURCE_DIR}/lib/platforminputcontexts/libibusplatforminputcontextplugin.so
    )  

  list(APPEND COMPILE_OPTION -fPIC)
elseif(WIN32)
  list(APPEND LINKLIBS
    debug ${PROJECT_SOURCE_DIR}/lib/Qt5Cored.lib optimized ${PROJECT_SOURCE_DIR}/lib/Qt5Core.lib
    debug ${PROJECT_SOURCE_DIR}/lib/Qt5Guid.lib optimized ${PROJECT_SOURCE_DIR}/lib/Qt5Gui.lib
    debug ${PROJECT_SOURCE_DIR}/lib/Qt5Widgetsd.lib optimized ${PROJECT_SOURCE_DIR}/lib/Qt5Widgets.lib		
    )
elseif(APPLE)
  # Set Manually 
  # set(Qt5_DIR ~/Qt/5.12.3/clang_64/lib/cmake/Qt5)
  # find_package(Qt5 COMPONENTS Core Gui Widgets )
  # list(APPEND Qt5::Core Qt5::Gui Qt5::Widgets)
endif()

include_directories(
  include/QtCore
  include/QtGui
  include/QtWidgets
  )

################################################

list(APPEND SRC
  )
list(APPEND INCL
  )

if(WIN32 AND MSVC)
  list(APPEND LINKLIBS)
  list(APPEND COMPILE_OPTION)
else()
  list(APPEND LINKLIBS
    -lm
    )
  list(APPEND COMPILE_OPTION
	    #gdb debugging option
	    -g
    )
endif()




################################################

add_executable(${PROJECT_NAME} ${SRC})

target_include_directories(${PROJECT_NAME}
  PRIVATE
  ${INCL}
  )

target_link_libraries(${PROJECT_NAME} ${LINKLIBS})
target_compile_options(${PROJECT_NAME} PRIVATE ${COMPILE_OPTION} )
set_target_properties(${PROJECT_NAME} PROPERTIES  CXX_STANDARD 14 )


