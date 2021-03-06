# Part of HTTPP.
#
# Distributed under the 2-clause BSD licence (See LICENCE.TXT file at the
# project root).
#
# Copyright (c) 2013 Thomas Sanchez.  All rights reserved.
#


ADD_DEFINITIONS("-DBOOST_TEST_MAIN")

IF (${BUILD_SHARED_LIBS})
    ADD_DEFINITIONS("-DBOOST_TEST_DYN_LINK=1")
ENDIF()

MACRO(ADD_HTTPP_TEST TEST_NAME)
    SET(EXE_NAME Test_${MODULE}_${TEST_NAME})
    # current project
    INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR}/httpp/include)
    ADD_EXECUTABLE(${EXE_NAME} ${TEST_NAME}.cpp)

    SET(LIBRARIES httpp ${ARGN} ${DEFAULT_LIBRARIES})
    TARGET_LINK_LIBRARIES(${EXE_NAME} ${LIBRARIES})

    SET(TEST_NAME ${MODULE}_${TEST_NAME})
    ADD_TEST(${TEST_NAME} ${EXE_NAME})

    SET_TESTS_PROPERTIES(${TEST_NAME} PROPERTIES
                         FAIL_REGULAR_EXPRESSION "FAILED")
    SET_TESTS_PROPERTIES(${TEST_NAME} PROPERTIES TIMEOUT 5)

ENDMACRO(ADD_HTTPP_TEST TEST_NAME)

# from: http://stackoverflow.com/questions/7787823/cmake-how-to-get-the-name-of-all-subdirectories-of-a-directory
MACRO(SUBDIRLIST result curdir)
    FILE(GLOB children RELATIVE ${curdir} ${curdir}/*)
    SET(dirlist "")
    FOREACH(child ${children})
        IF(IS_DIRECTORY ${curdir}/${child})
            SET(dirlist ${dirlist} ${child})
        ENDIF()
    ENDFOREACH()
    SET(${result} ${dirlist})
ENDMACRO()

SUBDIRLIST(SUBDIRS ${CMAKE_CURRENT_SOURCE_DIR})

FOREACH(subdir ${SUBDIRS})
        ADD_SUBDIRECTORY(${subdir})
ENDFOREACH()

