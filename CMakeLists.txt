cmake_minimum_required(VERSION 3.12)

project(CDF VERSION 1.0.0.0
  DESCRIPTION "CDF Build"
  LANGUAGES C CXX)

set(CMAKE_PLATFORM_INDEPENDENT_CODE ON)

set(cdf_src_dir ${CMAKE_CURRENT_SOURCE_DIR}/cdf38_0-dist/src)
set(cdf_src_lib_dir ${cdf_src_dir}/lib)
set(cdf_src_zlib_dir ${cdf_src_lib_dir}/zlib)
set(cdf_src_tools_dir ${cdf_src_dir}/tools)

set(cdf_include ${cdf_src_dir}/include)
set(cdf_zlib_include ${cdf_src_zlib_dir})

set(cdf_target cdf)

list(APPEND cdf_zlib_source
  ${cdf_src_zlib_dir}/adler32.c
  ${cdf_src_zlib_dir}/crc32.c
  ${cdf_src_zlib_dir}/deflate.c
  ${cdf_src_zlib_dir}/inffast.c
  ${cdf_src_zlib_dir}/inflate.c
  ${cdf_src_zlib_dir}/infback.c
  ${cdf_src_zlib_dir}/inftrees.c
  ${cdf_src_zlib_dir}/trees.c
  ${cdf_src_zlib_dir}/zutil.c)

list(APPEND cdf_source
  ${cdf_src_lib_dir}/cdflib.c
  ${cdf_src_lib_dir}/cdfcre.c
  ${cdf_src_lib_dir}/cdfope.c
  ${cdf_src_lib_dir}/cdfdel.c
  ${cdf_src_lib_dir}/cdfclo.c
  ${cdf_src_lib_dir}/cdfget.c
  ${cdf_src_lib_dir}/cdfput1.c
  ${cdf_src_lib_dir}/cdfput2.c
  ${cdf_src_lib_dir}/cdfsel.c
  ${cdf_src_lib_dir}/cdfcon.c
  ${cdf_src_lib_dir}/cdfwrite.c
  ${cdf_src_lib_dir}/cdfread.c
  ${cdf_src_lib_dir}/cdfmisc1.c
  ${cdf_src_lib_dir}/cdfmisc2.c
  ${cdf_src_lib_dir}/cdfmisc3.c
  ${cdf_src_lib_dir}/cdfalloc.c
  ${cdf_src_lib_dir}/cdftext.c
  ${cdf_src_lib_dir}/cdfcsi.c
  ${cdf_src_lib_dir}/cdffsi.c
  ${cdf_src_lib_dir}/cdffsi2.c
  ${cdf_src_lib_dir}/cdffsi3.c
  ${cdf_src_lib_dir}/cdffii.c
  ${cdf_src_lib_dir}/cdfstr.c
  ${cdf_src_lib_dir}/cdfed.c
  ${cdf_src_lib_dir}/cdfvalid.c
  ${cdf_src_lib_dir}/cdfhyper.c
  ${cdf_src_lib_dir}/cdfhyper64.c
  ${cdf_src_lib_dir}/cdfmem.c
  ${cdf_src_lib_dir}/cdfhuff.c
  ${cdf_src_lib_dir}/cdfrle.c
  ${cdf_src_lib_dir}/cdfgzip.c
  ${cdf_src_lib_dir}/epochu.c
  ${cdf_src_lib_dir}/epochuf.c
  ${cdf_src_lib_dir}/dirutils.c
  ${cdf_src_lib_dir}/vstream.c
  ${cdf_src_lib_dir}/cdfalloc64.c
  ${cdf_src_lib_dir}/cdfclo64.c
  ${cdf_src_lib_dir}/cdfcon64.c
  ${cdf_src_lib_dir}/cdfcre64.c
  ${cdf_src_lib_dir}/cdfdel64.c
  ${cdf_src_lib_dir}/cdfget64.c
  ${cdf_src_lib_dir}/cdfput1_64.c
  ${cdf_src_lib_dir}/cdfput2_64.c
  ${cdf_src_lib_dir}/cdfmisc1_64.c
  ${cdf_src_lib_dir}/cdfmisc2_64.c
  ${cdf_src_lib_dir}/cdfmisc3_64.c
  ${cdf_src_lib_dir}/cdfread64.c
  ${cdf_src_lib_dir}/cdfsel64.c
  ${cdf_src_lib_dir}/cdfwrite64.c
  ${cdf_src_lib_dir}/vstream64.c
  ${cdf_src_lib_dir}/cdfhuff64.c
  ${cdf_src_lib_dir}/cdfrle64.c
  ${cdf_src_lib_dir}/cdfgzip64.c
  ${cdf_src_lib_dir}/cdfcks.c
  ${cdf_src_lib_dir}/cdfmd5.c
  ${cdf_src_lib_dir}/cdfsav.c
  ${cdf_src_lib_dir}/cdfvalidator.c
  ${cdf_src_lib_dir}/cdfvalidator64.c
  ${cdf_src_lib_dir}/cdftt2000.c
  ${cdf_src_lib_dir}/cdftt2000f.c)

list(APPEND cdf_public_header
  ${cdf_include}/cdf.h
  ${cdf_include}/cdfconfig.h)

list(APPEND cdf_definitions
  _FILE_OFFSET_BITS=64
  ALONE)

add_library(${cdf_target}
  ${cdf_source} ${cdf_zlib_source})

target_include_directories(${cdf_target} PUBLIC
  ${cdf_include} ${cdf_zlib_include})

target_compile_definitions(${cdf_target} PUBLIC
  ${cdf_definitions})

set_target_properties(${cdf_target}
  PROPERTIES PUBLIC_HEADER
  "${cdf_public_header}")

install(TARGETS ${cdf_target}
  LIBRARY DESTINATION lib
  PUBLIC_HEADER DESTINATION include
  ARCHIVE DESTINATION lib)

set(cdfdump_target cdfdump)

add_executable(${cdfdump_target}
  ${cdf_src_tools_dir}/cdfdump.c
  ${cdf_src_tools_dir}/toolbox1.c
  ${cdf_src_tools_dir}/toolbox3.c
  ${cdf_src_tools_dir}/toolbox4.cpp)

target_include_directories(${cdfdump_target} PRIVATE
  ${cdf_include})

target_compile_definitions(${cdfdump_target} PUBLIC
  ${cdf_definitions})

target_link_libraries(${cdfdump_target}
  ${cdf_target})

install(TARGETS ${cdfdump_target}
  RUNTIME DESTINATION bin)
