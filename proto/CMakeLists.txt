set(PROTO_FILES
    actuators.proto
    altimeter.proto
    battery_state.proto
    bezier_curve.proto
    bezier_trajectory.proto
    can.proto
    header.proto
    imu.proto
    joy.proto
    led.proto
    led_array.proto
    magnetic_field.proto
    nav_sat_fix.proto
    nav_sat_status.proto
    odometry.proto
    pixy_vector.proto
    point.proto
    pose.proto
    pose_with_covariance.proto
    quaternion.proto
    safety.proto
    sim_clock.proto
    status.proto
    time.proto
    twist.proto
    twist_with_covariance.proto
    vector3.proto
    wheel_odometry.proto
    )

set(PROTO_SRCS)
set(PROTO_HDRS)

protobuf_generate(
    APPEND_PATH
    LANGUAGE cpp
    OUT_VAR outvar
    IMPORT_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/../..
    PROTOS ${PROTO_FILES})

foreach(_file ${outvar})
    if (_file MATCHES "cc$")
        list(APPEND PROTO_SRCS ${_file})
    elseif (_file MATCHES "h$")
        list(APPEND PROTO_HDRS ${_file})
    elseif (_file MATCHES "hpp$")
        list(APPEND PROTO_HDRS ${_file})
    endif()
endforeach()

message(STATUS "proto_src: ${PROTO_SRCS}")
message(STATUS "proto_hdrs: ${PROTO_HDRS}")

add_library(${PROJECT_NAME}
  ${PROTO_SRCS}
  ${PROTO_HDRS}
)

ament_target_dependencies(${PROJECT_NAME} Protobuf) 

target_include_directories(${PROJECT_NAME}
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/../..>
        $<INSTALL_INTERFACE:include>)

install(
    TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)

install(FILES
    ${PROTO_HDRS}
    DESTINATION include/${PROJECT_NAME}
)

# vi: et ts=4 sw=4
