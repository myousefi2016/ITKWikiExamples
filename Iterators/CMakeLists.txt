PROJECT (Iterators)

IF(NOT ITKWikiExamples_BINARY_DIR)
FIND_PACKAGE(ITK REQUIRED)
INCLUDE(${ITK_USE_FILE})
FIND_PACKAGE(VTK REQUIRED)
INCLUDE(${VTK_USE_FILE})
ENDIF(NOT ITKWikiExamples_BINARY_DIR)

include(${ITKWikiExamples_SOURCE_DIR}/CMake/RequiresVersion.cmake)

#
# Build all .cxx files in the directory
FILE(GLOB ALL_FILES *.cxx)

Requires_Version(ShapedNeighborhoodIterator 4.0 ALL_FILES)

foreach(SOURCE_FILE ${ALL_FILES})
  STRING(REPLACE ".cxx" "" TMP ${SOURCE_FILE})
  STRING(REPLACE ${CMAKE_CURRENT_SOURCE_DIR}/ "" EXAMPLE ${TMP})
  ADD_EXECUTABLE(${WIKI}${EXAMPLE} MACOSX_BUNDLE ${EXAMPLE}.cxx)
  TARGET_LINK_LIBRARIES(${WIKI}${EXAMPLE} ${ItkVtkGlue} ${ITK_LIBRARIES})
endforeach(SOURCE_FILE)

if(BUILD_TESTING)
# Testing
SET(KIT Iterators)
SET(KIT_LIBS ${ItkVtkGlue} ${ITK_LIBRARIES})
SET(NEEDS_ARGS
  ConstNeighborhoodIterator
  ImageRegionConstIterator
  ImageRegionConstIteratorWithIndex
  ImageRegionIterator
  ImageRegionIteratorWithIndex
  NeighborhoodIterator
)

SET(DATA ${ITKWikiExamples_SOURCE_DIR}/Testing/Data)
SET(TEMP ${ITKWikiExamples_BINARY_DIR}/Testing/Temporary)

ADD_TEST(${KIT}-ConstNeighborhoodIterator ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/${KIT}CxxTests
  TestConstNeighborhoodIterator ${DATA}/cthead1.png)

ADD_TEST(${KIT}-ImageRegionConstIterator ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/${KIT}CxxTests
  TestImageRegionConstIterator ${DATA}/cthead1.png)

ADD_TEST(${KIT}-ImageRegionConstIteratorWithIndex ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/${KIT}CxxTests
  TestImageRegionConstIteratorWithIndex ${DATA}/cthead1.png)

ADD_TEST(${KIT}-ImageRegionIterator ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/${KIT}CxxTests
  TestImageRegionIterator ${DATA}/cthead1.png)

ADD_TEST(${KIT}-ImageRegionIteratorWithIndex ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/${KIT}CxxTests
  TestImageRegionIteratorWithIndex ${DATA}/cthead1.png)

ADD_TEST(${KIT}-NeighborhoodIterator ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/${KIT}CxxTests
  TestNeighborhoodIterator ${DATA}/cthead1.png)

INCLUDE(${ITKWikiExamples_SOURCE_DIR}/CMake/ExamplesTesting.cmake)

endif()
