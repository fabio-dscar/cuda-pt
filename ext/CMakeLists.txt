cmake_minimum_required (VERSION 3.20)

include(FetchContent)

# ---------------------------------------------------------------------------------------
#     OpenGL
# ---------------------------------------------------------------------------------------
find_package(OpenGL REQUIRED)

set(OPENGL_LIBRARIES ${OPENGL_LIBRARIES} PARENT_SCOPE)

# ---------------------------------------------------------------------------------------
#     glm
# ---------------------------------------------------------------------------------------
set(BUILD_TESTING OFF CACHE BOOL " " FORCE)
FetchContent_Declare(
  glm
  GIT_REPOSITORY https://github.com/g-truc/glm.git
  GIT_TAG        586a402397dd35d66d7a079049856d1e2cbab300
  FIND_PACKAGE_ARGS
)

FetchContent_MakeAvailable(glm)

if(GLM_FOUND)
  set(GLM_INCLUDE_DIR ${GLM_INCLUDE_DIRS} PARENT_SCOPE)
  set(GLM_LIBRARIES ${GLM_LIBRARIES} PARENT_SCOPE)
else()
  set(GLM_INCLUDE_DIR ${glm_SOURCE_DIR} PARENT_SCOPE)
  set(GLM_LIBRARIES glm PARENT_SCOPE)
endif()