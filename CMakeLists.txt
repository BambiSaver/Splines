############################################################################
#                                                                          #
#  file: CMakeLists.txt                                                    #
#  Copyright (C) 2016                                                      #
#      Enrico Bertolazzi                                                   #
#      Dipartimento di Ingegneria Industriale                              #
#      Universita` degli Studi di Trento                                   #
#      email: enrico.bertolazzi@ing.unitn.it                               #
#                                                                          #
############################################################################

cmake_minimum_required(VERSION 3.5)

#SET( TARGET Splines )

add_compile_options(-DSPLINES_DO_NOT_USE_GENERIC_CONTAINER)


PROJECT( ${TARGET} CXX C )

SET( CMAKE_ALLOW_LOOSE_LOOP_CONSTRUCTS TRUE )
SET( CMAKE_VERBOSE_MAKEFILE            TRUE )
SET( CMAKE_BUILD_TYPE                  Release )

IF( CMAKE_COMPILER_IS_GNUCXX OR CMAKE_COMPILER_IS_GNUCC )
  SET( CMAKE_CXX_FLAGS "-Wall -Wcast-align" )
  IF ( APPLE )
    SET( CMAKE_CXX_FLAGS_RELEASE "-O3 -g0 -funroll-loops " )
    SET( CMAKE_CXX_FLAGS_DEBUG   "-O1 -g -DDEBUG " )
  ELSE()
    SET( CMAKE_CXX_FLAGS_RELEASE "-fPIC -fno-common -O3 -g0 -funroll-loops " )
    SET( CMAKE_CXX_FLAGS_DEBUG   "-fPIC -fno-common -O1 -g -DDEBUG " )
  ENDIF()
ELSEIF( MSVC )
  SET( VSFLAGS_COMMON "/nologo /GS /W3 /WX- /Gm- /Gd /fp:precise /EHsc /FS /D_WINDOWS /D_CRT_SECURE_NO_DEPRECATE /D_CRT_SECURE_NO_WARNINGS /DHAVE_STRING_H /DNO_GETTIMEOFDAY" )
  SET( CMAKE_CXX_FLAGS_RELEASE "/O2 /MD ${VSFLAGS_COMMON}" )
  SET( CMAKE_CXX_FLAGS_DEBUG   "/Od /Ob0 /MDd /Zi /RTC1 /D_DEBUG ${VSFLAGS_COMMON}" )
ENDIF()

SET( CMAKE_C_FLAGS         ${CMAKE_CXX_FLAGS} )
SET( CMAKE_C_FLAGS_RELEASE ${CMAKE_CXX_FLAGS_RELEASE} )
SET( CMAKE_C_FLAGS_DEBUG   ${CMAKE_CXX_FLAGS_DEBUG} )

SET( SOURCES )
FILE( GLOB S ./src/*.cc )
FOREACH (F ${S})
  FILE( RELATIVE_PATH RF ${CMAKE_CURRENT_SOURCE_DIR} "${F}" )
  LIST( APPEND SOURCES ${RF} )
ENDFOREACH (F ${S})
SET( HEADERS )
FILE( GLOB S ./src/*.hh )
FOREACH (F ${S})
  FILE( RELATIVE_PATH RF ${CMAKE_CURRENT_SOURCE_DIR} "${F}" )
  LIST( APPEND HEADERS ${RF} )
ENDFOREACH (F ${S})





# MAKE A CATKIN PACKAGE

#cmake_minimum_required(VERSION 2.8.3)
project(splines)
find_package(catkin REQUIRED)
catkin_package(
  INCLUDE_DIRS src
  LIBRARIES Splines
#  CATKIN_DEPENDS other_catkin_pkg
#  DEPENDS system_lib
)



###########
## Build ##
###########

# Declare a C++ library
add_library(${PROJECT_NAME}
    ${SOURCES}
)

include_directories(
   include
   ${catkin_INCLUDE_DIRS}
   src
   # DO NOT USE GenericContainer GC/lib/include
)


#############
## Install ##
#############

install(TARGETS ${PROJECT_NAME}
   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
 )

install(DIRECTORY src/
   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
   FILES_MATCHING PATTERN "*.hh"
   #${HEADERS}
 )


#############
## Testing ##
#############

# Add gtest based cpp test target and link libraries
catkin_add_gtest(${PROJECT_NAME}-test1 tests/test1.cc)
catkin_add_gtest(${PROJECT_NAME}-test2 tests/test2.cc)
catkin_add_gtest(${PROJECT_NAME}-test3 tests/test3.cc)
catkin_add_gtest(${PROJECT_NAME}-test4 tests/test4.cc)
catkin_add_gtest(${PROJECT_NAME}-test5 tests/test5.cc)
catkin_add_gtest(${PROJECT_NAME}-test6 tests/test6.cc)
catkin_add_gtest(${PROJECT_NAME}-test7 tests/test7.cc)


if(TARGET ${PROJECT_NAME}-test1)
  target_link_libraries(${PROJECT_NAME}-test1 ${PROJECT_NAME})
endif()
 if(TARGET ${PROJECT_NAME}-test2)
  target_link_libraries(${PROJECT_NAME}-test2 ${PROJECT_NAME})
endif()
 if(TARGET ${PROJECT_NAME}-test3)
  target_link_libraries(${PROJECT_NAME}-test3 ${PROJECT_NAME})
endif()
 if(TARGET ${PROJECT_NAME}-test4)
  target_link_libraries(${PROJECT_NAME}-test4 ${PROJECT_NAME})
endif()
 if(TARGET ${PROJECT_NAME}-test5)
  target_link_libraries(${PROJECT_NAME}-test5 ${PROJECT_NAME})
endif()
 if(TARGET ${PROJECT_NAME}-test6)
  target_link_libraries(${PROJECT_NAME}-test6 ${PROJECT_NAME})
endif()
 if(TARGET ${PROJECT_NAME}-test7)
  target_link_libraries(${PROJECT_NAME}-test7 ${PROJECT_NAME})
endif()

# Add folders to be run by python nosetests

catkin_add_nosetests(tests)
