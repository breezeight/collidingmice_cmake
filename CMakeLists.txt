CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
PROJECT(CollidingMice)
 
################################################# QT4 Handling
FIND_PACKAGE(Qt4 REQUIRED)

#this include set all the qt variable need to compile qt 
INCLUDE(${QT_USE_FILE})

#Add qt definitions
ADD_DEFINITIONS(${QT_DEFINITIONS})

#Put here all headers files that need moc 
SET( Qt4_HEADERS
)

#Put here .ui form files
SET( Qt4_UI
)

#Put here .qrc resource files
SET( Qt4_RES mice.qrc
)

#Cmake macro 
QT4_WRAP_CPP(MOC_CPP ${Qt4_HEADERS})
QT4_WRAP_UI(UI_HEADERS ${Qt4_UI})
QT4_ADD_RESOURCES(RES_CPP ${Qt4_RES})

#With this line, all intermediary files will be available in the include path.
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_BINARY_DIR})

################################################# CPack
INCLUDE(InstallRequiredSystemLibraries)

SET(CPACK_PACKAGE_DESCRIPTION_SUMMARY "My funky project")
SET(CPACK_PACKAGE_VENDOR "Me, myself, and I")
#SET(CPACK_PACKAGE_DESCRIPTION_FILE "${CMAKE_CURRENT_SOURCE_DIR}/ReadMe.txt")
#SET(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/Copyright.txt")
SET(CPACK_PACKAGE_VERSION_MAJOR "1")
SET(CPACK_PACKAGE_VERSION_MINOR "3")
SET(CPACK_PACKAGE_VERSION_PATCH "2")
SET(CPACK_PACKAGE_INSTALL_DIRECTORY "CMake ${CMake_VERSION_MAJOR}.${CMake_VERSION_MINOR}")
IF(WIN32 AND NOT UNIX)
  # There is a bug in NSI that does not handle full unix paths properly. Make
  # sure there is at least one set of four (4) backlasshes.
  SET(CPACK_PACKAGE_ICON "${CMake_SOURCE_DIR}/Utilities/Release\\\\InstallIcon.bmp")
  SET(CPACK_NSIS_INSTALLED_ICON_NAME "bin\\\\MyExecutable.exe")
  SET(CPACK_NSIS_DISPLAY_NAME "${CPACK_PACKAGE_INSTALL_DIRECTORY} My Famous Project")
  SET(CPACK_NSIS_HELP_LINK "http:\\\\\\\\www.my-project-home-page.org")
  SET(CPACK_NSIS_URL_INFO_ABOUT "http:\\\\\\\\www.my-personal-home-page.com")
  SET(CPACK_NSIS_CONTACT "me@my-personal-home-page.com")
  SET(CPACK_NSIS_MODIFY_PATH ON)
ELSE(WIN32 AND NOT UNIX)
  SET(CPACK_STRIP_FILES "bin/MyExecutable")
  SET(CPACK_SOURCE_STRIP_FILES "")
ENDIF(WIN32 AND NOT UNIX)
SET(CPACK_PACKAGE_EXECUTABLES "MyExecutable" "My Executable")

INCLUDE(CPack)

################################################# General
SET(CMAKE_BUILD_TYPE Release)

SET(CMAKE_CXX_FLAGS "-Wall")

SET( SRC
	main.cpp
	mouse.cpp
	${MOC_CPP}
	${UI_HEADERS} #Added here only to add a dependency between the TARGET and the ui compilation
	${RES_CPP}
)
 
SET( LIB
	${QT_LIBRARIES}
)
 
ADD_EXECUTABLE(collidingmice ${SRC})
TARGET_LINK_LIBRARIES( collidingmice ${LIB} )

