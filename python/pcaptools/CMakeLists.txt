# Copyright 2013 Dominic Spill
#
# This file is part of Libbtbb (pcapdump)
#
# GNU Radio is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2, or (at your option)
# any later version.
#
# GNU Radio is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with GNU Radio; see the file COPYING.  If not, write to
# the Free Software Foundation, Inc., 51 Franklin Street,
# Boston, MA 02110-1301, USA.

set(SETUP_PY_IN ${CMAKE_CURRENT_SOURCE_DIR}/setup.py.in)
set(SETUP_PY    ${CMAKE_CURRENT_BINARY_DIR}/setup.py)
set(DEPS        ${CMAKE_CURRENT_SOURCE_DIR}/pcapdump/__init__.py
                ${CMAKE_CURRENT_SOURCE_DIR}/pcapdump/pcapdump.py)
set(OUTPUT      ${CMAKE_CURRENT_BINARY_DIR}/build)

configure_file(${SETUP_PY_IN} ${SETUP_PY})

add_custom_command(OUTPUT ${OUTPUT}/timestamp
                   COMMAND ${PYTHON_EXECUTABLE} setup.py build
                   COMMAND ${CMAKE_COMMAND} -E touch ${OUTPUT}/timestamp
                   DEPENDS ${DEPS})

add_custom_target(pcapdump ALL DEPENDS ${OUTPUT}/timestamp)
install(CODE "execute_process(COMMAND ${PYTHON_EXECUTABLE} ${SETUP_PY} build -b ${OUTPUT} install --prefix=${BUILD_ROOT}/${CMAKE_INSTALL_PREFIX})")

add_custom_target(btaptap ALL DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/btaptap)
install(PROGRAMS btaptap DESTINATION ${INSTALL_DEFAULT_BINDIR})

