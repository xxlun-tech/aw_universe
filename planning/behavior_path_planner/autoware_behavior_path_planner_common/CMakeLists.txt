cmake_minimum_required(VERSION 3.14)
project(autoware_behavior_path_planner_common)

find_package(autoware_cmake REQUIRED)
autoware_package()

find_package(magic_enum CONFIG REQUIRED)

ament_auto_add_library(${PROJECT_NAME} SHARED
  src/turn_signal_decider.cpp
  src/interface/scene_module_visitor.cpp
  src/interface/scene_module_interface.cpp
  src/interface/scene_module_manager_interface.cpp
  src/utils/utils.cpp
  src/utils/path_utils.cpp
  src/utils/traffic_light_utils.cpp
  src/utils/path_safety_checker/safety_check.cpp
  src/utils/path_safety_checker/objects_filtering.cpp
  src/utils/path_shifter/path_shifter.cpp
  src/utils/drivable_area_expansion/static_drivable_area.cpp
  src/utils/drivable_area_expansion/drivable_area_expansion.cpp
  src/utils/drivable_area_expansion/map_utils.cpp
  src/utils/drivable_area_expansion/footprints.cpp
  src/utils/parking_departure/geometric_parallel_parking.cpp
  src/utils/parking_departure/utils.cpp
  src/utils/occupancy_grid_based_collision_detector/occupancy_grid_based_collision_detector.cpp
  src/marker_utils/utils.cpp
)

target_include_directories(${PROJECT_NAME} SYSTEM PUBLIC
  ${EIGEN3_INCLUDE_DIR}
)

if(BUILD_TESTING)
  ament_add_ros_isolated_gmock(test_${PROJECT_NAME}_utilities
    test/test_utils.cpp
    test/test_path_utils.cpp
    test/test_traffic_light_utils.cpp
  )

  target_link_libraries(test_${PROJECT_NAME}_utilities
  ${PROJECT_NAME}
  )

  ament_target_dependencies(test_${PROJECT_NAME}_utilities
    autoware_test_utils
  )

  ament_add_ros_isolated_gmock(test_${PROJECT_NAME}_drivable_area_expansion
    test/test_drivable_area_expansion.cpp
    test/test_footprints.cpp
    test/test_static_drivable_area.cpp
  )

  target_link_libraries(test_${PROJECT_NAME}_drivable_area_expansion
    ${PROJECT_NAME}
  )

  ament_target_dependencies(test_${PROJECT_NAME}_drivable_area_expansion
    autoware_test_utils
  )

  ament_add_ros_isolated_gmock(test_${PROJECT_NAME}_occupancy_grid_based_collision_detector
    test/test_occupancy_grid_based_collision_detector.cpp
  )

  target_link_libraries(test_${PROJECT_NAME}_occupancy_grid_based_collision_detector
    ${PROJECT_NAME}
  )

  ament_target_dependencies(test_${PROJECT_NAME}_occupancy_grid_based_collision_detector
    autoware_test_utils
  )

  ament_add_ros_isolated_gmock(test_${PROJECT_NAME}_safety_check
    test/test_safety_check.cpp
    test/test_objects_filtering.cpp
  )

  target_link_libraries(test_${PROJECT_NAME}_safety_check
    ${PROJECT_NAME}
  )

  ament_target_dependencies(test_${PROJECT_NAME}_safety_check
    autoware_test_utils
  )

  ament_add_ros_isolated_gmock(test_${PROJECT_NAME}_parking_departure
    test/test_parking_departure_utils.cpp
  )

  target_link_libraries(test_${PROJECT_NAME}_parking_departure
    ${PROJECT_NAME}
  )

  ament_target_dependencies(test_${PROJECT_NAME}_parking_departure
    autoware_test_utils
  )

  ament_add_ros_isolated_gmock(test_${PROJECT_NAME}_path_shifter
    test/test_path_shifter.cpp
  )

  target_link_libraries(test_${PROJECT_NAME}_path_shifter
    ${PROJECT_NAME}
  )

  ament_target_dependencies(test_${PROJECT_NAME}_path_shifter
    autoware_test_utils
  )

  ament_add_ros_isolated_gmock(test_${PROJECT_NAME}_turn_signal
    test/test_turn_signal.cpp
  )

  target_link_libraries(test_${PROJECT_NAME}_turn_signal
    ${PROJECT_NAME}
  )
endif()

ament_auto_package(INSTALL_TO_SHARE
  test_data
)
