# ########################################################################
# Copyright 2013 Advanced Micro Devices, Inc.
# 
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
# 
# http://www.apache.org/licenses/LICENSE-2.0
# 
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# ########################################################################


#  client
set( Client.Source	client.cpp 
                    openCL.misc.cpp 
                    stdafx.cpp )

set( Client.Headers client.h 
                    openCL.misc.h 
                    ../statTimer/statisticalTimer.extern.h
                    ../include/unicode.compatibility.h 
                    ../include/stdafx.h 
                    ../include/targetver.h 
                    ../include/clFFT.h )

set( Client.Files ${Client.Source} ${Client.Headers} )

set( DL_LIB "" )
if( WIN32 )
	add_definitions( "/D_CONSOLE" )
else()
	# To use the dlopen() and dlclose() functions, we should link with libdl
	set( DL_LIB "-ldl" )
endif()

# Include standard OpenCL headers
include_directories( ${Boost_INCLUDE_DIRS} ${OPENCL_INCLUDE_DIRS} ../../../common ${PROJECT_BINARY_DIR}/include ../include )

add_executable( Client ${Client.Files} )

target_link_libraries( Client clFFT ${Boost_LIBRARIES} ${OPENCL_LIBRARIES} ${DL_LIB} )

set_target_properties( Client PROPERTIES VERSION ${CLFFT_VERSION} )
set_target_properties( Client PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}/staging" )

# CPack configuration; include the executable into the package
install( TARGETS Client
        RUNTIME DESTINATION bin${SUFFIX_BIN}
        LIBRARY DESTINATION lib${SUFFIX_LIB}
        ARCHIVE DESTINATION lib${SUFFIX_LIB}/import
        )