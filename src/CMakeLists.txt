cmake_minimum_required(VERSION 2.0)

set(TargetName "luckybtc")

project(${TargetName})

set(BUILD "debug")

if(${BUILD} STREQUAL "debug")
    set(D "d")
    set(DEBUGFLAGS "/Zi /Od /D__WXDEBUG__ /D_DEBUG")
endif()

set(WXDEFS "/DWIN32 /D__WXMSW__ /D_WINDOWS /DNOPCH")
set(CFLAGS "/c /nologo /Ob0 /MD${D} /EHsc /GR /Zm300 /YX /w" ${DEBUGFLAGS} ${WXDEFS})
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /SUBSYSTEM:WINDOWS")
string(REPLACE " /Zm1000" "" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")

set(LIBS libdb47s${D}.lib libeay32.lib wxmsw28${D}_richtext.lib wxmsw28${D}_html.lib wxmsw28${D}_core.lib
    wxbase28${D}.lib wxtiff${D}.lib wxjpeg${D}.lib wxpng${D}.lib wxzlib${D}.lib wxregex${D}.lib wxexpat${D}.lib
    kernel32.lib user32.lib gdi32.lib comdlg32.lib winspool.lib winmm.lib shell32.lib comctl32.lib ole32.lib
    oleaut32.lib uuid.lib rpcrt4.lib advapi32.lib ws2_32.lib
    )

include_directories(
    C:/boost
    C:/DB/build_windows
    C:/OpenSSL/include
    C:/wxWidgets/lib/vc_lib/mswd
    C:/wxWidgets/include
    )

link_directories(
    /DB/build_windows/${BUILD}
    /OpenSSL/out32
    /wxWidgets/lib/vc_lib
    )

set(HEADERS headers.h util.h main.h serialize.h uint256.h key.h bignum.h script.h db.h base58.h)

set(SRC_LIST 
    db.cpp
    irc.cpp
    main.cpp
    market.cpp
    net.cpp
    script.cpp
    sha.cpp
    ui.cpp
    uibase.cpp
    util.cpp
    ui.rc
)

add_executable(${TargetName} ${SRC_LIST} ${HEADERS})

set_target_properties(${TargetName} PROPERTIES COMPILE_FLAGS ${CFLAGS})

target_link_libraries(${TargetName} ${LIBS})