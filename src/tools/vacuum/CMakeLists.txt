
OMESSAGE("antares-vacuum")

# Le main
set(SRCS
	main.cpp
	modified-inode.h
	modified-inode.cpp
	antares-study.cpp
	antares-study.h
	io.h
	io.cpp
)



if(WIN32)
	FILE(REMOVE "${CMAKE_CURRENT_SOURCE_DIR}/win32/vacuum.o")
	CONFIGURE_FILE("${CMAKE_CURRENT_SOURCE_DIR}/win32/vacuum.rc.cmake"
		"${CMAKE_CURRENT_SOURCE_DIR}/win32/vacuum.rc")
	SET(SRCS ${SRCS} "win32/vacuum.rc")
endif(WIN32)


set(execname "antares-${ANTARES_PRG_VERSION}-vacuum")
add_executable(${execname}  ${SRCS})

set(VACUUM_LIBS
	libantares-core
	libantares-license
	libantares-fswalker
	yuni-static-core
	${CMAKE_THREADS_LIBS_INIT}
)

if (WIN32 OR WIN64)
	target_link_libraries(${execname} ${VACUUM_LIBS})
else()
	target_link_libraries(${execname} -Wl,--start-group ${VACUUM_LIBS} -Wl,--end-group)
endif()


import_std_libs(${execname})
executable_strip(${execname})

