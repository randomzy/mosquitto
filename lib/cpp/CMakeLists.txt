set(CPP_SRC mosquittopp.cpp mosquittopp.h)

add_library(mosquittopp SHARED ${CPP_SRC})
target_include_directories(mosquittopp PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
set_target_properties(mosquittopp PROPERTIES
	POSITION_INDEPENDENT_CODE 1
)
target_link_libraries(mosquittopp libmosquitto)
set_target_properties(mosquittopp PROPERTIES
	VERSION ${VERSION}
	SOVERSION 1
)
install(TARGETS mosquittopp
	RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
	ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
	LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}")

if (WITH_STATIC_LIBRARIES)
	add_library(mosquittopp_static STATIC
		${CPP_SRC}
	)
	if (WITH_PIC)
		set_target_properties(mosquittopp_static PROPERTIES
			POSITION_INDEPENDENT_CODE 1
		)
	endif (WITH_PIC)

	target_link_libraries(mosquittopp_static libmosquitto_static)
	target_include_directories(mosquittopp_static PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

	set_target_properties(mosquittopp_static PROPERTIES
		OUTPUT_NAME mosquittopp_static
		VERSION ${VERSION}
	)

	target_compile_definitions(mosquittopp_static PUBLIC "LIBMOSQUITTO_STATIC")
	install(TARGETS mosquittopp_static ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}")
endif (WITH_STATIC_LIBRARIES)

install(FILES mosquittopp.h DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}")
