add_library(sharedtools OBJECT "")

list(APPEND SHAREDTOOLS_INCLUDES
   ${CMAKE_BINARY_DIR}/privateinclude/QtCore/private
   ${CMAKE_BINARY_DIR}/privateinclude/QtXml/private

   ${PROJECT_SOURCE_DIR}/src/tools/shared
)

list(APPEND SHAREDTOOLS_HEADERS
    ${PROJECT_SOURCE_DIR}/src/tools/shared/translator.h
    ${PROJECT_SOURCE_DIR}/src/tools/shared/translatormessage.h
)

target_sources(sharedtools
   PRIVATE
   ${PROJECT_SOURCE_DIR}/src/tools/shared/numerus.cpp
   ${PROJECT_SOURCE_DIR}/src/tools/shared/translator.cpp
   ${PROJECT_SOURCE_DIR}/src/tools/shared/translatormessage.cpp

   ${PROJECT_SOURCE_DIR}/src/tools/shared/qm.cpp
   ${PROJECT_SOURCE_DIR}/src/tools/shared/ts.cpp
)

function_variable_fixup("${SHAREDTOOLS_EXTRA_CXXFLAGS}" SHAREDTOOLS_EXTRA_CXXFLAGS)
function_variable_fixup("${SHAREDTOOLS_EXTRA_LDFLAGS}"  SHAREDTOOLS_EXTRA_LDFLAGS)

target_compile_definitions(sharedtools PRIVATE ${SHAREDTOOLS_DEFINITIONS})
target_include_directories(sharedtools PRIVATE ${SHAREDTOOLS_INCLUDES})

target_link_libraries(sharedtools
   CsCore
   CsXml
)

set_target_properties(sharedtools
   PROPERTIES
   COMPILE_FLAGS ${SHAREDTOOLS_EXTRA_CXXFLAGS}
   LINK_FLAGS ${SHAREDTOOLS_EXTRA_LDFLAGS}
)
