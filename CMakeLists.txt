cmake_minimum_required(VERSION 2.8)

project(mms_sample_ext)

file(GLOB SOURCES src/*.cpp)

set(SMSDK "${PROJECT_SOURCE_DIR}/external/sourcemod")
set(HL2SDK "${PROJECT_SOURCE_DIR}/external/hl2sdk-csgo")
set(MMSOURCE "${PROJECT_SOURCE_DIR}/external/metamod-source")
set(HL2PUB "${HL2SDK}/public")
set(METAMOD "${MMSOURCE}/core")
set(LIB_PREFIX "lib")

if(APPLE)
    set(LIB_EXT "dylib")
    set(HL2LIB "${HL2SDK}/lib/mac")
    set(CXX_FLAGS_LIST
        -DOSX
        -D_OSX
    )
    set(LINKER_FLAGS_LIST
        -dynamiclib
        -lstdc++
        -mmacosx-version-min=10.5
    )
elseif(UNIX)
    set(LIB_EXT "so")
    set(HL2LIB "${HL2SDK}/lib/linux")
    set(CXX_FLAGS_LIST
        -D_LINUX
    )
    set(LINKER_FLAGS_LIST
        -shared
        -lstdc++
    )
    if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
        list(APPEND LINKER_FLAGS_LIST -static-libgcc)
    endif()
endif()

if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
    list(APPEND CXX_FLAGS_LIST -mfpmath=sse)
endif()

set(LIB_SUFFIX ".${LIB_EXT}")

set(CXX_OPT_FLAGS_LIST
    -DNDEBUG
    -O3
    -funroll-loops
    -pipe
    -fno-strict-aliasing
)

set(CXX_DEBUG_FLAGS_LIST
    -D_DEBUG
    -DDEBUG
    -g
    -ggdb3
)

list(APPEND CXX_FLAGS_LIST
    -DSOURCE_ENGINE=12
    -fvisibility=hidden
    -fvisibility-inlines-hidden
    -std=c++11
    -DPOSIX
    -Dstricmp=strcasecmp
    -D_stricmp=strcasecmp
    -D_strnicmp=strncasecmp
    -Dstrnicmp=strncasecmp
    -D_snprintf=snprintf
    -D_vsnprintf=vsnprintf
    -D_alloca=alloca
    -Dstrcmpi=strcasecmp
    -DCOMPILER_GCC
    -Wall
    -Werror
    -Wno-overloaded-virtual
    -Wno-switch
    -Wno-unused
    -Wno-non-virtual-dtor
    -Wno-delete-non-virtual-dtor
    -fno-exceptions
    -fno-rtti
    -Wno-deprecated-register
    -msse
    -DSOURCEMOD_BUILD
    -DHAVE_STDINT_H
    -m32
    -DSE_EPISODEONE=1
    -DSE_DARKMESSIAH=2
    -DSE_ORANGEBOX=3
    -DSE_BLOODYGOODTIME=4
    -DSE_EYE=5
    -DSE_CSS=6
    -DSE_ORANGEBOXVALVE=7
    -DSE_LEFT4DEAD=8
    -DSE_LEFT4DEAD2=9
    -DSE_ALIENSWARM=10
    -DSE_PORTAL2=11
    -DSE_CSGO=12
)

list(APPEND LINKER_FLAGS_LIST
    -m32
    -ldl
    -lm
)

string(REPLACE ";" " " CXX_FLAGS "${CXX_FLAGS_LIST}")
string(REPLACE ";" " " LINKER_FLAGS "${LINKER_FLAGS_LIST}")
string(REPLACE ";" " " CXX_DEBUG_FLAGS "${CXX_DEBUG_FLAGS_LIST}")
string(REPLACE ";" " " CXX_OPT_FLAGS "${CXX_OPT_FLAGS_LIST}")

if(CMAKE_BUILD_TYPE EQUAL Debug)
    set(CXX_FLAGS "${CXX_FLAGS} ${CXX_DEBUG_FLAGS}")
endif(CMAKE_BUILD_TYPE EQUAL Debug)

if(CMAKE_BUILD_TYPE EQUAL Release)
    set(CXX_FLAGS "${CXX_FLAGS} ${CXX_OPT_FLAGS}")
endif(CMAKE_BUILD_TYPE EQUAL Release)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${CXX_FLAGS}")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} ${LINKER_FLAGS}")

add_library(${PROJECT_NAME} SHARED ${SOURCES})

include_directories(
        ${HL2SDK}/public/game/server
        ${HL2PUB}
        ${HL2PUB}/engine
        ${HL2PUB}/tier0
        ${HL2PUB}/tier1
        ${METAMOD}
        ${METAMOD}/sourcehook
        ${SMSDK}
        ${SMSDK}/public
        ${SMSDK}/public/jit
        ${SMSDK}/public/jit/x86
        ${SMSDK}/extensions
        ${SMSDK}/sourcepawn
        ${SMSDK}/sourcepawn/include
)

target_link_libraries(${PROJECT_NAME}
        ${HL2LIB}/tier1_i486.a
        ${HL2LIB}/interfaces_i486.a
        ${HL2LIB}/${LIB_PREFIX}vstdlib${LIB_SUFFIX}
        ${HL2LIB}/${LIB_PREFIX}tier0${LIB_SUFFIX}
)

set_target_properties(${PROJECT_NAME} PROPERTIES PREFIX "")

#get_cmake_property(_variableNames VARIABLES)
#foreach(_variableName ${_variableNames})
#    message(STATUS "${_variableName}=${${_variableName}}")
#endforeach()