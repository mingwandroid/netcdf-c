# This cmake file is here as a placeholder.
# It cannot yet be used to run the filter test.

if(ENABLE_FILTER_TEST)
SET(BZIP2SRC blocksort.c huffman.c crctable.c randtable.c compress.c decompress.c bzlib.c)
ADD_EXECUTABLE(fake fake.c H5Zbzip2.c ${BZIP2SRC})

SET(BZIP2OBJ blocksort.o huffman.o crctable.o randtable.o compress.o decompress.o bzlib.o)

SET(LIBNAME "lib${FILTERNAME}.so")

SET(bzip__LIBS H5Zbzip2.o ${BZIP2OBJ})
FOREACH(LIBS ${bzip_LIBS})
  SET(LARGS ${LARGS} $<TARGET_OBJECTS:${LIBS}>)
ENDFOREACH()
ADD_LIBRARY(bzip2 ${LARGS} )
SET(TLL_LIBS "")
SET(TLL_LIBS ${TLL_LIBS} ${HDF5_hdf5_hl_LIBRARY} ${HDF5_hdf5_LIBRARY} ${ZLIB_LIBRARY})
TARGET_LINK_LIBRARIES(bzip2 ${TLL_LIBS})
SET_TARGET_PROPERTIES(bzip2 PROPERTIES LINK_FLAGS -shared)

build_bin_test(test_filter)
IF(BUILD_UTILITIES)
  ADD_SH_TEST(nc_test4 tst_filter)
ENDIF(BUILD_UTILITIES)

ENDIF(ENABLE_FILTER_TEST)

FILE(GLOB COPY_FILES ${CMAKE_CURRENT_SOURCE_DIR}/*)
FILE(COPY ${COPY_FILES} DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/ FILE_PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE)

FILE(GLOB CUR_EXTRA_DIST RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/*)
SET(CUR_EXTRA_DIST ${CUR_EXTRA_DIST} CMakeLists.txt Makefile.am)
ADD_EXTRA_DIST("${CUR_EXTRA_DIST}")
