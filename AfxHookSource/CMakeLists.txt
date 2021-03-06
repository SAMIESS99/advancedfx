cmake_minimum_required (VERSION 3.8)

project ("afxhooksource")

set(AFXHOOKSOURCE_SHADERS
    "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_line_vs20.acs"
    "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_line_vs20.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_line_ps20.acs"
    "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_line_ps20.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_pgldraw_vs20.acs"
    "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_pgldraw_vs20.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_pgldraw_ps20.acs"
    "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_pgldraw_ps20.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_depth_ps20.acs"
    "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_depth_ps20.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_drawtexture_ps20.acs"
    "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_drawtexture_ps20.h"
)

set(AFXHOOKSOURCE_SHADERS_SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/afx_line_vs20.fxc"
	"${CMAKE_CURRENT_SOURCE_DIR}/../shaders/afx_line_ps20.fxc"
	"${CMAKE_CURRENT_SOURCE_DIR}/../shaders/afx_pgldraw_vs20.fxc"
	"${CMAKE_CURRENT_SOURCE_DIR}/../shaders/afx_pgldraw_ps20.fxc"
	"${CMAKE_CURRENT_SOURCE_DIR}/../shaders/afx_depth_ps20.fxc"
	"${CMAKE_CURRENT_SOURCE_DIR}/../shaders/afx_drawtexture_ps20.fxc"
)

add_custom_command(
    WORKING_DIRECTORY ${shaderbuilder_BINARY_DIR}
    OUTPUT ${AFXHOOKSOURCE_SHADERS}
    DEPENDS shaderbuilder ${AFXHOOKSOURCE_SHADERS_SOURCES}
    COMMAND ShaderBuilder.exe --profile vs_2_0 --in "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/afx_line_vs20.fxc" --outPrefix "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_line_vs20"
	COMMAND ShaderBuilder.exe --profile ps_2_0 --in "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/afx_line_ps20.fxc" --outPrefix "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_line_ps20"
	COMMAND ShaderBuilder.exe --profile vs_2_0 --in "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/afx_pgldraw_vs20.fxc" --outPrefix "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_pgldraw_vs20"
	COMMAND ShaderBuilder.exe --profile ps_2_0 --in "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/afx_pgldraw_ps20.fxc" --outPrefix "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_pgldraw_ps20"
	COMMAND ShaderBuilder.exe --profile ps_2_0 --in "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/afx_depth_ps20.fxc" --outPrefix "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_depth_ps20"
	COMMAND ShaderBuilder.exe --profile ps_2_0 --in "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/afx_drawtexture_ps20.fxc" --outPrefix "${CMAKE_CURRENT_SOURCE_DIR}/../shaders/build/afx_drawtexture_ps20"	
)

add_custom_target(afxhooksourceshaders
    DEPENDS ${AFXHOOKSOURCE_SHADERS}
)

add_custom_target(afxhooksource
    DEPENDS ilmbase_build openexr_build afxhooksourceshaders
    WORKING_DIRECTORY ${afxhooksource_BINARY_DIR}
    BYPRODUCTS "${afxhooksource_BINARY_DIR}/AfxHookSource.dll"
    COMMAND ${VS16_MSBUILD} "${afxhooksource_SOURCE_DIR}/AfxHookSource.vcxproj" "/p:Configuration=Release" "/p:Platform=Win32" "/p:OutDir=${afxhooksource_BINARY_DIR}/" "/p:ILMBASE_BUILD_DIR=${ILMBASE_BUILD_DIR};OPENEXR_BUILD_DIR=${OPENEXR_BUILD_DIR}"
)
