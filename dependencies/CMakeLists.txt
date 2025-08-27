find_package(OpenGL REQUIRED)

include(FetchContent)

# ------------
# --- GLAD ---
# ------------

FetchContent_Declare(
    glad 
    GIT_REPOSITORY https://github.com/Dav1dde/glad.git 
    GIT_TAG v0.1.36)
FetchContent_MakeAvailable(glad)

# ------------
# --- GLFW ---
# ------------

set(GLFW_BUILD_DOCS     OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS    OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set(GLFW_INSTALL        OFF CACHE BOOL "" FORCE)

FetchContent_Declare(
    glfw 
    GIT_REPOSITORY https://github.com/glfw/glfw.git 
    GIT_TAG 3.4)
FetchContent_MakeAvailable(glfw)

# -------------
# --- ImGUI ---
# -------------

FetchContent_Declare(
    imgui
    GIT_REPOSITORY https://github.com/ocornut/imgui.git
    GIT_TAG v1.92.2)
FetchContent_GetProperties(imgui)

if (NOT imgui_POPULATED)
    FetchContent_Populate(imgui)

    set(IMGUI_INCLUDE_DIR ${imgui_SOURCE_DIR}/)
    file(GLOB IMGUI_SOURCES ${imgui_SOURCE_DIR}/*.cpp)
    file(GLOB IMGUI_HEADERS ${imgui_SOURCE_DIR}/*.h)

    add_library(imgui STATIC 
        ${IMGUI_SOURCES}
        ${imgui_SOURCE_DIR}/backends/imgui_impl_glfw.cpp 
        ${imgui_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp)

    target_compile_definitions(imgui PUBLIC IMGUI_IMPL_OPENGL_LOADER_GLAD)
    
    target_include_directories(imgui 
        PUBLIC ${IMGUI_INCLUDE_DIR} 
        PUBLIC ${GLFW_INCLUDE_DIR} 
        PUBLIC ${GLAD_INCLUDE_DIR})

    target_link_libraries(imgui 
    PRIVATE OpenGL::GL 
    PRIVATE glfw 
    PRIVATE glad)
endif ()