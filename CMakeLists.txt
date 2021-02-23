cmake_minimum_required(VERSION 3.5)
project(unitree_legged_real)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED
        #        controller_manager
        #        unitree_legged_msgs
        )
find_package(geometry_msgs REQUIRED)
find_package(tf2 REQUIRED)
find_package(rclcpp REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(unitree_legged_msgs REQUIRED)

set(dependencies
        rclcpp
        tf2
        geometry_msgs
        Eigen3
        unitree_legged_msgs
        )

include_directories(
        include
        ${Boost_INCLUDE_DIR}
        #        ${EIGEN3_INCLUDE_DIR}
        ${LEGGED_INCLUDE_PATH}
        $ENV{UNITREE_LEGGED_SDK_PATH}/include
)

#add_executable(walk_operator src/walk_operators.cpp)
#ament_target_dependencies(walk_operator
#        ${dependencies}
#        )
#target_link_libraries(walk_operator
#        ${LEGGED_SDK_NAME}
#        )

link_directories($ENV{UNITREE_LEGGED_SDK_PATH}/lib)
string(CONCAT LEGGED_SDK_NAME libunitree_legged_sdk_$ENV{UNITREE_PLATFORM}.so)
set(EXTRA_LIBS ${LEGGED_SDK_NAME} lcm)

add_executable(walk_instruction src/walk_instruction.cpp)
#add_dependencies(walk_operator ${${PEOJECT_NAME}_EXPORTED_TARGETS} ${ament_EXPORTED_TARGETS})
ament_target_dependencies(walk_instruction
        ${dependencies}
        )
target_link_libraries(walk_instruction
        ${EXTRA_LIBS}
        )

add_executable(walk_client src/walk_client.cpp)
#add_dependencies(walk_operator ${${PEOJECT_NAME}_EXPORTED_TARGETS} ${ament_EXPORTED_TARGETS})
ament_target_dependencies(walk_client
        ${dependencies}
        )
#target_link_libraries(walk_client
#        ${LEGGED_SDK_NAME}
#        )

#add_executable(send_msg src/send_msg.cpp)
#target_link_libraries(send_msg ${LEGGED_SDK_NAME})
#add_dependencies(send_msg ${${PEOJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

ament_export_include_directories(include)
ament_export_dependencies(${dependencies})

install(TARGETS
        walk_client
		walk_instruction
        DESTINATION lib/${PROJECT_NAME}
)

ament_package()

