# Copyright (C) 2015 Hewlett-Packard Development Company, L.P.
# All Rights Reserved.
#
#    Licensed under the Apache License, Version 2.0 (the "License"); you may
#    not use this file except in compliance with the License. You may obtain
#    a copy of the License at
#
#         http://www.apache.org/licenses/LICENSE-2.0
#
#    Unless required by applicable law or agreed to in writing, software
#    distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
#    WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
#    License for the specific language governing permissions and limitations
#    under the License.

cmake_minimum_required (VERSION 2.8)

set (POWERD powerd)
project (${POWERD})

# Rules to locate needed libraries
include(FindPkgConfig)
pkg_check_modules(SYSTEMD libsystemd)
pkg_check_modules(CONFIG_YAML config-yaml)
pkg_check_modules(HALONUTILS halonutils)
pkg_check_modules(OVSCOMMON libovscommon)
pkg_check_modules(OVSDB libovsdb)

# Sources to build powerd
set (SOURCES powerd.c)

include_directories (${PROJECT_BINARY_DIR} ${PROJECT_SOURCE_DIR}
                     ${OVSCOMMON_INCLUDE_DIRS}
)

# Rules to build powerd
add_executable (${POWERD} ${SOURCES})

target_link_libraries (${POWERD} ${OVSCOMMON_LIBRARIES} ${OVSDB_LIBRARIES} ${SYSTEMD_LIBRARIES}
                       ${CONFIG_YAML_LIBRARIES} ${SYSTEMD_LIBRARIES} ${HALONUTILS_LIBRARIES}
                       -lpthread -lrt -lm)

install(TARGETS ${POWERD}
	RUNTIME DESTINATION bin)

