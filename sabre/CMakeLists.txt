cmake_minimum_required(VERSION 3.16)

# list the header files
set(HEADER_FILES
	include/sabre/Tkn.h
	include/sabre/Token_List.h
	include/sabre/Unit.h
	include/sabre/Err.h
	include/sabre/Scan.h
	include/sabre/Utils.h
	include/sabre/AST.h
	include/sabre/AST_Printer.h
	include/sabre/Type_Interner.h
	include/sabre/Check.h
	include/sabre/GLSL.h
	include/sabre/Reflect.h
	include/sabre/Expr_Value.h
	include/sabre/HLSL.h
	include/sabre/Scope.h
	include/sabre/IR.h
	include/sabre/SPIRV.h
	include/sabre/IR_Text.h
)

# list the source files
set(SOURCE_FILES
	src/sabre/Err.cpp
	src/sabre/Unit.cpp
	src/sabre/Scan.cpp
	src/sabre/Utils.cpp
	src/sabre/Parse.cpp
	src/sabre/AST_Printer.cpp
	src/sabre/Type_Interner.cpp
	src/sabre/Check.cpp
	src/sabre/GLSL.cpp
	src/sabre/Reflect.cpp
	src/sabre/Expr_Value.cpp
	src/sabre/HLSL.cpp
	src/sabre/Scope.cpp
	src/sabre/IR.cpp
	src/sabre/SPIRV.cpp
	src/sabre/IR_Text.cpp
)

add_library(sabre)
add_library(MoustaphaSaad::sabre ALIAS sabre)

target_sources(sabre
	PRIVATE ${HEADER_FILES}
			${SOURCE_FILES})

if (SABRE_UNITY_BUILD)
	set_target_properties(sabre
		PROPERTIES UNITY_BUILD_BATCH_SIZE 0
				   UNITY_BUILD true)
endif()

target_link_libraries(sabre
	PUBLIC
		MoustaphaSaad::mn
)

target_compile_definitions(sabre
	PRIVATE
		SABRE_LOG_METRICS=$<BOOL:${SABRE_LOG_METRICS}>
)

# enable C++17
# disable any compiler specifc extensions
# add d suffix in debug mode
target_compile_features(sabre PUBLIC cxx_std_17)
set_target_properties(sabre PROPERTIES
	CXX_EXTENSIONS OFF
	DEBUG_POSTFIX d
)

# generate exports header file
include(GenerateExportHeader)
generate_export_header(sabre
	EXPORT_FILE_NAME ${CMAKE_CURRENT_SOURCE_DIR}/include/sabre/Exports.h
)

# list include directories
target_include_directories(sabre
	PUBLIC
	${CMAKE_CURRENT_SOURCE_DIR}/include
	PRIVATE
	${CMAKE_CURRENT_SOURCE_DIR}/src
)

if (SABRE_INSTALL)
	install(TARGETS sabre)
	install(DIRECTORY include/sabre DESTINATION include)
endif (SABRE_INSTALL)