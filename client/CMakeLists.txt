cmake_minimum_required (VERSION 3.10)
project (SimpleEngine)

find_package(CXBase)
find_package(SOIL)
find_package(GLEW CONFIG)
find_package(glfw3)
find_package(freetype CONFIG)
find_package(IMGui)
find_package(luadbg CONFIG)

file(GLOB_RECURSE  SOURCES_CPP "src/*.cpp")
file(GLOB_RECURSE  SOURCES_HEADER_PRI "src/*.h"  "src/*.hpp" "src/*.inl")
file(GLOB_RECURSE  PRIVATE_HEADERS "src/*.h" "src/*.hpp" "src/*.inl")

source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR}/src PREFIX "Source Files"  FILES ${SOURCES_CPP})
source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR}/src PREFIX "Source Files"  FILES ${SOURCES_HEADER_PRI})

add_executable(SimpleEngine
    ${PRIVATE_HEADERS}
    src/actor/action.cpp
    src/actor/actor.cpp
    src/actor/player.cpp
    src/actor/player_state.cpp
    src/algorithm.cpp
    src/animation/frame_animation.cpp
    src/astar/astar.cpp
    src/bitmap.cpp
    src/camera.cpp
    src/combat/combat.cpp
    src/combat/skill.cpp
    src/components/base_component.cpp
    src/components/transform_component.cpp
    src/core/graphics_2d.cpp
    src/core/object_2d.cpp
    src/core/renderer.cpp
    src/core/shader.cpp
    src/core/texture.cpp
    src/core/transform.cpp
    src/cube_batch.cpp
    src/engine.cpp
    src/entity.cpp
    src/file_system.cpp
    src/game.cpp
    src/imgui/imgui_impl_glfw.cpp
    src/imgui/imgui_impl_opengl3.cpp
    src/input_manager.cpp
    src/logger.cpp
    src/message.cpp
    src/net.cpp
    src/file_loading.cpp
    src/objects/map_object.cpp
    src/objects/object.cpp
    src/profile.cpp
    src/main.cpp
    src/resource_manager.cpp
    src/scene/base_scene.cpp
    src/scene/game_map.cpp
    src/scene/scene.cpp
    src/scene/scene_manager.cpp
    src/scene/splash_scene.cpp
    src/scene/ui_scene.cpp
    src/scene/was_viewer_scene.cpp
    src/script_system.cpp
    src/sprite_batch.cpp
    src/sprite_renderer.cpp
    src/state.cpp
    src/test_scene.cpp
    src/text_renderer.cpp
    src/texture_manager.cpp
    src/time/time.cpp
    src/time/time_manager.cpp
    src/time/timer.cpp
    src/time/timer_manager.cpp
    src/tiny_thread_pool.cpp
    src/ui.cpp
    src/vertex.cpp
    src/window.cpp
)

if(MSVC)
    target_compile_options(SimpleEngine PRIVATE /wd4819 )
    
#   target_compile_options(SimpleEngine PRIVATE /W4 /WX /wd4324 /wd4201 /wd4819 /wd4267)
else()
#   target_compile_options(SimpleEngine PRIVATE -Wall -Wextra -pedantic -Werror)
endif(MSVC)

target_include_directories(SimpleEngine
    PUBLIC
        $<INSTALL_INTERFACE:include>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
        ${CMAKE_CURRENT_SOURCE_DIR}/../external/dist/include
        ${CMAKE_CURRENT_SOURCE_DIR}/../external/dist/include/freetype2
)


target_compile_definitions(SimpleEngine 
  PRIVATE
    -DTW_STATIC
    -DTW_NO_LIB_PRAGMA
    -DTW_NO_DIRECT3D
    -DGLEW_STATIC
    -D_CRT_SECURE_NO_WARNINGS
	-DIMGUI_IMPL_OPENGL_LOADER_GLEW
    -DWORK_DIR="${ENGINE_DIR}"
    -DNODEFAULTLIB
)
if(WIN32)
target_compile_definitions(SimpleEngine 
  PRIVATE
    -DZLIB_WINAPI
    -DNOMINMAX
    -DUNICODE
    -D_UNICODE
    -DWIN32_LEAN_AND_MEAN
    -D_WIN32_WINNT=0x0501
)
endif(WIN32)

set_target_properties(SimpleEngine PROPERTIES COTIRE_CXX_PREFIX_HEADER_INIT "src/pch/precompile.h")
cotire(SimpleEngine)

target_link_libraries(SimpleEngine
  PRIVATE 
    glfw
    GLEW
    SOIL
    freetype
    CXBase::CXBase
    IMGui::IMGui
    luadbg
)

# add_custom_command(TARGET SimpleEngine
#     PRE_LINK
#     COMMAND ${CMAKE_COMMAND} -E copy ${ENGINE_DIR}/externals/dist/bin/luadbg.dll ${ENGINE_DIR}/bin/$<CONFIG>/
# )
