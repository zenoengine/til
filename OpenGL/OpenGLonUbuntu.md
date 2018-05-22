# Ubuntu에서 OpenGL 프로그래밍 환경 구축하기

우분투에서 OpenGL을 이용해서 빨간 창을 띄우는 방법을 소개해드리려 합니다.

# 1. Ubuntu를 설치하세요

Ubuntu 홈페이지에서 Ubuntu를 받아 USB에 부트디스크를 만들어 우분투를 설치합니다.

검색을 해보면 많은 사람들이 이 방법에 대해 설명을 해놓았으니 참고하시면 좋겠습니다.

# 2. 종속성 다운로드

처음 시작하는 사람들에게 openGL은 interface라는것을 아는것이 중요합니다.

opengl에서 제공해주는 여러 라이브러리들(예) glew) 그래픽스 프로그래밍을 편리하도록 함수(인터페이스)를

제공해주는 것이지요.

이제 이 관련 라이브러리들(종속성)을 다운 받습니다.

터미널을 실행시키고 다음 명령어를 실행합니다.   

```bash
sudo apt-get install cmake libx11-dev xorg-dev libglu1-mesa-dev freeglut3-dev libglew1.5 libglew1.5-dev libglu1-mesa libglu1-mesa-dev libgl1-mesa-glx libgl1-mesa-devsudo apt-get install cmake libx11-dev xorg-dev libglu1-mesa-dev freeglut3-dev libglew1.5 libglew1.5-dev libglu1-mesa libglu1-mesa-dev libgl1-mesa-glx libgl1-mesa-dev libglfw3-dev libglfw3
```

# 3. 개발 코드 예제 작성

이제 라이브러리들을 사용해서 예제 코드를 작성해봅시다.

아래 코드를 복사해서 main.cpp를 하나 만들어줍시다.

아래 코드는 작고 빨간 배경을 가진 윈도우를 그려주는 코드입니다.

```c
#include <GL/glew.h>
#include <GL/glu.h>
#include <GLFW/glfw3.h>
#include <iostream>

static void error_callback(int error, const char* description)
{
    fputs(description, stderr);
}

static void key_callback(GLFWwindow* window, int key, int scancode, int action, int mods)
{
    if (key == GLFW_KEY_ESCAPE && action == GLFW_PRESS)
    glfwSetWindowShouldClose(window, GL_TRUE);
}

int main(void)
{
    GLFWwindow* window;
    static const GLfloat red[] = {1.0f, 0.0f, 0.0f, 1.0f};

    glfwSetErrorCallback(error_callback);

    if (!glfwInit()) {
        exit(EXIT_FAILURE);
    }

    window = glfwCreateWindow(640, 480, "Simple example", NULL, NULL);

    if (!window)
    {
        glfwTerminate();
        exit(EXIT_FAILURE);
    }

    glfwMakeContextCurrent(window);
    glfwSetKeyCallback(window, key_callback);

    glewExperimental=GL_TRUE;
    GLenum err=glewInit();
    if(err!=GLEW_OK)
    {
        //Problem: glewInit failed, something is seriously wrong.
        std::cout<<"glewInit failed, aborting."<<std::endl;
    }

    while (!glfwWindowShouldClose(window))
    {
        glClearBufferfv(GL_COLOR, 0, red);

        glfwSwapBuffers(window);
        glfwPollEvents();
    }

    glfwDestroyWindow(window);
    glfwTerminate();

    exit(EXIT_SUCCESS);
}
```

그리고 makefile을 작성합시다.

```makefile
# OBJS specifies which files to compile as part of the project
OBJS = main.cpp

# CC specifies which compiler we're using
CC = g++ -std=c++11

# COMPILER_FLAGS specifies the additional compilation options we're using
# -Wall will turn on all standard warnings
COMPILER_FLAGS = -Wall

# LINKER_FLAGS specifies the libraries we're linking against
LINKER_FLAGS = -lGL -lGLU -lglut -lGLEW -lglfw -lX11 -lXxf86vm -lXrandr -lpthread -lXi -ldl -lXinerama -lXcursor

# OBJ_NAME specifies the name of our exectuable
OBJ_NAME = main

#This is the target that compiles our executable
all: $(OBJS)
$(CC) $(OBJS) $(COMPILER_FLAGS) $(LINKER_FLAGS) -o $(OBJ_NAME)
```

# 4. 빌드 및 실행!

```bash
make && ./main
```

이후 실행된 프로그램을 볼 수 있습니다 :)