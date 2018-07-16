CMAKE_MINIMUM_REQUIRED(VERSION 3.11)

project(zlib)

SET(zlib_sources
    adler32.c
    compress.c
    crc32.c
    crc32.h
    crypt.h
    deflate.c
    deflate.h
    gzio.c
    infback.c
    inffast.c
    inffast.h
    inffixed.h
    inflate.c
    inflate.h
    inftrees.c
    inftrees.h
    ioapi.h
    trees.c
    trees.h
    uncompr.c
    unzip.c
    unzip.h
    zconf.h
    zconf.in.h
    zlib.h
    zutil.c
    zutil.h)

SOURCE_GROUP(zlib FILES ${zlib_sources})
ADD_LIBRARY(zlib STATIC ${zlib_sources})

IF(MSVC)
    ADD_DEFINITIONS(/D_CRT_SECURE_NO_WARNINGS)
ENDIF()

INSTALL(TARGETS zlib ARCHIVE DESTINATION lib)

INSTALL(FILES zlib.h zutil.h zconf.h crc32.h crypt.h deflate.h inffast.h inffixed.h inftrees.h ioapi.h
        DESTINATION include)

SET(ZLIB_LIBRARIES zlib)
SET(ZLIB_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR} CACHE INTERNAL "Zlib path")

TARGET_INCLUDE_DIRECTORIES(zlib PUBLIC ${ZLIB_INCLUDE_DIR})