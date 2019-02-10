# Copyright (c) 2017 The Bitcoin developers

cmake_minimum_required(VERSION 3.13)


# Add path for custom modules
set(CMAKE_MODULE_PATH
	${CMAKE_MODULE_PATH}
	${CMAKE_CURRENT_SOURCE_DIR}/cmake/modules
)

include("cmake/modules/HunterGate.cmake")

option(HUNTER_STATUS_DEBUG "Hunter debug info" OFF)
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.23.34.tar.gz"
	SHA1 "70287b1ffa810ee4e952052a9adff9b4856d0d54"
	LOCAL
)

project(BitcoinSV)

hunter_add_package(Boost COMPONENTS chrono filesystem program_options thread test system)
hunter_add_package(Libevent)
hunter_add_package(ZeroMQ)
hunter_add_package(OpenSSL)

# If ccache is available, then use it.
find_program(CCACHE ccache)
if(CCACHE)
	set_property(GLOBAL PROPERTY RULE_LAUNCH_COMPILE ${CCACHE})
	set_property(GLOBAL PROPERTY RULE_LAUNCH_LINK ${CCACHE})
endif(CCACHE)

# Add the magic taret check and check-all
add_custom_target(check-all)
add_custom_target(check)

add_subdirectory(src)
add_subdirectory(test)
