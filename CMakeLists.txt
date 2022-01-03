cmake_minimum_required(VERSION 3.0.2) ## 운영체제에 설치되어 있는 cmake의 최소 버전
project(my_first_ros_pkg) ## 패키지의 이름

## catkin build 시에 요구되는 구성요소 패키지
## 입력된 패키지가 없을 경우 사용자에게 에러가 표시된다.
## 사용자가 만든 패키지가 의존하는 패키지를 먼저 설치하게 만드는 옵션이다.
find_package(catkin REQUIRED COMPONENTS 
  roscpp
  std_msgs
)


## catkin build 옵션
## "INCLUDE_DIRS" 뒤에 설정한 패키지 내부 폴더인 "include"의 헤더파일을 사용하겠다는 설정
## "LIBRARIES"는 뒤에 설정한 패키지의 라이브러리를 사용하겠다는 설정
## "CATKIN_DEPENDS" 캐킨 빌드 시 의존하는 패키지들. 아래는 roscpp std_msgs가 의존하고 있다는 설정.
## "DEPENDS" 시스템 의존 패키지를 기술하는 설정
catkin_package(

  INCLUDE_DIRS include
  CATKIN_DEPENDS roscpp std_msgs
  DEPENDS system_lib

)

## include 폴더를 지정할 수 있는 옵션.
## 현재 ${catkin_INCLUDE_DIRS}라고 설정하고 있음. 
## 각 패키지 안의 "include" 폴더를 의미하고 이 안의 헤더 파일을 이용한다는 설정
include_directories(${catkin_INCLUDE_DIRS})

## cpp실행 파일을 선언한다. hello_world_node.cpp 파일을 참조하여 hello_world_node 실행 파일 생성
add_executable(hello_world_node src/hello_world_node.cpp)

## 패키지 빌드에 앞서 생성해야할 메시지 헤더파일이 있을 경우 빌드 전에 우선적으로 메시지를 생성하라는 설정
## my_first_ros_pkg_generate_messages_cpp를 우선적으로 빌드하고 hello_world_node를 빌드하게 하는 설정
add_dependencies(hello_world_node my_first_ros_pkg_generate_messages_cpp)

## hello_world_node 를 생성하기 앞서서 링크 해야하는 라이브러리 및 실행파일을 링크해주는 옵
target_link_libraries(hello_world_node ${catkin_LIBRARIES})

