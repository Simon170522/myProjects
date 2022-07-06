cmake_minimum_required(VERSION 3.20)
project(Symdiff_GL)    #имя функции, не забудь поменять везде

set(CMAKE_CXX_STANDARD 14)
set(OpenGlLinkers -lGLU -lGL -lglut ) # setting all the Glut libraries as one variable.


add_executable(Symdiff_GL graphics.cpp)
include_directories(GL)

target_link_libraries(Symdiff_GL ${OPENGL_LIBRARIES} ${GLEW_LIBRARIES} ${GLUT_LIBRARIES} ${OpenGlLinkers})
