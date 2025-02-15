#==============================================================================
#  @@-COPYRIGHT-START-@@
#  
#  Copyright (c) 2018, Qualcomm Innovation Center, Inc. All rights reserved.
#  
#  Redistribution and use in source and binary forms, with or without 
#  modification, are permitted provided that the following conditions are met:
#  
#  1. Redistributions of source code must retain the above copyright notice, 
#     this list of conditions and the following disclaimer.
#  
#  2. Redistributions in binary form must reproduce the above copyright notice, 
#     this list of conditions and the following disclaimer in the documentation 
#     and/or other materials provided with the distribution.
#  
#  3. Neither the name of the copyright holder nor the names of its contributors 
#     may be used to endorse or promote products derived from this software 
#     without specific prior written permission.
#  
#  THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" 
#  AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE 
#  IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE 
#  ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE 
#  LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR 
#  CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF 
#  SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS 
#  INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN 
#  CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) 
#  ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
#  POSSIBILITY OF SUCH DAMAGE.
#  
#  SPDX-License-Identifier: BSD-3-Clause
#  
#  @@-COPYRIGHT-END-@@
#==============================================================================


include_directories(BEFORE
        ${CMAKE_CURRENT_SOURCE_DIR}/../src
        )

add_executable(MoDlEqualizationTest
        TestDLEqualization.cpp
        )

target_compile_options(MoDlEqualizationTest
        PRIVATE
            -DUSE_OPENCV
        )

target_include_directories(MoDlEqualizationTest
        PRIVATE ${OPENCV_INCLUDE_DIRS}
        )

target_link_libraries(MoDlEqualizationTest
        MoDlEqualization
        stdc++
        ${OPENCV_LINK_LIBRARIES}
        z
        dl
        gtest_main
        )

add_test(NAME MoDlEqualizationTest
         COMMAND $<TARGET_FILE:MoDlEqualizationTest> --gtest_output=xml:cpp_test_output.xml)

add_test(NAME MoDlEqualizationPythonTest
      COMMAND ${Python3_EXECUTABLE} -m pytest ${CMAKE_CURRENT_SOURCE_DIR} --junitxml=${CMAKE_CURRENT_BINARY_DIR}/py_test_output.xml
      )

set_property(TEST MoDlEqualizationPythonTest
      APPEND PROPERTY ENVIRONMENT "${AIMET_PYTHONPATH}")
