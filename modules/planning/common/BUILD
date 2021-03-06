load("//tools:cpplint.bzl", "cpplint")

package(default_visibility = ["//visibility:public"])

cc_library(
    name = "indexed_list",
    hdrs = [
        "indexed_list.h",
    ],
    linkopts = [
        "-lboost_thread",
    ],
    deps = [
        "//modules/common/util:map_util",
    ],
)

cc_test(
    name = "indexed_list_test",
    size = "small",
    srcs = [
        "indexed_list_test.cc",
    ],
    deps = [
        ":indexed_list",
        "//modules/common/util",
        "@gtest//:main",
    ],
)

cc_library(
    name = "indexed_queue",
    hdrs = [
        "indexed_queue.h",
    ],
    deps = [
        "//modules/common/util:map_util",
    ],
)

cc_test(
    name = "indexed_queue_test",
    size = "small",
    srcs = [
        "indexed_queue_test.cc",
    ],
    deps = [
        ":indexed_queue",
        "//modules/common/util",
        "@gtest//:main",
    ],
)

cc_library(
    name = "obstacle",
    srcs = [
        "obstacle.cc",
    ],
    hdrs = [
        "obstacle.h",
    ],
    deps = [
        ":indexed_list",
        ":planning_gflags",
        "//modules/common/math:geometry",
        "//modules/common/util",
        "//modules/perception/proto:perception_proto",
        "//modules/planning/common:planning_context",
        "//modules/planning/proto:planning_proto",
        "//modules/prediction/proto:prediction_proto",
    ],
)

filegroup(
    name = "common_testdata",
    srcs = glob([
        "testdata/**",
    ]),
)

cc_test(
    name = "obstacle_test",
    size = "small",
    srcs = [
        "obstacle_test.cc",
    ],
    data = [
        "//modules/planning/common:common_testdata",
    ],
    deps = [
        ":obstacle",
        "//modules/common/util",
        "//modules/perception/proto:perception_proto",
        "@gtest//:main",
    ],
)

cc_library(
    name = "path_obstacle",
    srcs = [
        "path_obstacle.cc",
    ],
    hdrs = [
        "path_obstacle.h",
    ],
    deps = [
        ":indexed_list",
        ":obstacle",
        "//modules/common/configs:vehicle_config_helper",
        "//modules/common/util:map_util",
        "//modules/planning/common/speed:st_boundary",
        "//modules/planning/proto:planning_proto",
        "//modules/planning/reference_line",
    ],
)

cc_test(
    name = "path_obstacle_test",
    size = "small",
    srcs = [
        "path_obstacle_test.cc",
    ],
    deps = [
        ":path_obstacle",
        "@gtest//:main",
    ],
)

cc_library(
    name = "planning_context",
    srcs = [
        "planning_context.cc",
    ],
    hdrs = [
        "planning_context.h",
    ],
    deps = [
        ":planning_gflags",
        "//modules/common/adapters:adapter_manager",
        "//modules/planning/proto:planning_proto",
        "//modules/planning/proto:planning_status_proto",
    ],
)

cc_library(
    name = "path_decision",
    srcs = [
        "path_decision.cc",
    ],
    hdrs = [
        "path_decision.h",
    ],
    deps = [
        ":obstacle",
        ":path_obstacle",
        "//modules/planning/reference_line",
    ],
)

cc_library(
    name = "planning_gflags",
    srcs = [
        "planning_gflags.cc",
    ],
    hdrs = [
        "planning_gflags.h",
    ],
    deps = [
        "//external:gflags",
    ],
)

cc_library(
    name = "reference_line_info",
    srcs = [
        "reference_line_info.cc",
    ],
    hdrs = [
        "reference_line_info.h",
    ],
    deps = [
        ":ego_info",
        ":path_decision",
        ":planning_gflags",
        "//modules/common:log",
        "//modules/common/proto:pnc_point_proto",
        "//modules/common/util:thread_pool",
        "//modules/common/vehicle_state:vehicle_state_provider",
        "//modules/map/pnc_map",
        "//modules/map/proto:map_proto",
        "//modules/planning/common/path:path_data",
        "//modules/planning/common/speed:speed_data",
        "//modules/planning/common/trajectory:discretized_trajectory",
        "//modules/planning/common/trajectory:publishable_trajectory",
        "//modules/planning/proto:lattice_structure_proto",
        "//modules/planning/reference_line",
        "@eigen",
    ],
)

cc_test(
    name = "reference_line_info_test",
    size = "small",
    srcs = [
        "reference_line_info_test.cc",
    ],
    deps = [
        ":reference_line_info",
        "@gtest//:main",
    ],
)

cc_library(
    name = "speed_profile_generator",
    srcs = [
        "speed_profile_generator.cc",
    ],
    hdrs = [
        "speed_profile_generator.h",
    ],
    deps = [
        ":frame",
        ":reference_line_info",
        "//modules/common/proto:pnc_point_proto",
        "//modules/planning/common/speed:speed_data",
        "//modules/planning/proto:planning_config_proto",
    ],
)

cc_test(
    name = "speed_profile_generator_test",
    size = "small",
    srcs = [
        "speed_profile_generator_test.cc",
    ],
    deps = [
        ":speed_profile_generator",
        "@gtest//:main",
    ],
)

cc_library(
    name = "lag_prediction",
    srcs = [
        "lag_prediction.cc",
    ],
    hdrs = [
        "lag_prediction.h",
    ],
    deps = [
        ":planning_gflags",
        "//modules/common/adapters:adapter_manager",
        "//modules/prediction/proto:prediction_proto",
    ],
)

cc_library(
    name = "change_lane_decider",
    srcs = [
        "change_lane_decider.cc",
    ],
    hdrs = [
        "change_lane_decider.h",
    ],
    deps = [
        ":planning_gflags",
        ":reference_line_info",
        "//modules/map/pnc_map:route_segments",
        "//modules/planning/proto:planning_proto",
    ],
)

cc_library(
    name = "frame",
    srcs = [
        "frame.cc",
    ],
    hdrs = [
        "frame.h",
    ],
    deps = [
        ":change_lane_decider",
        ":indexed_queue",
        ":lag_prediction",
        ":obstacle",
        ":reference_line_info",
        "//modules/common:log",
        "//modules/common/adapters:adapter_manager",
        "//modules/common/configs:vehicle_config_helper",
        "//modules/common/monitor_log",
        "//modules/common/vehicle_state:vehicle_state_provider",
        "//modules/map/hdmap:hdmap_util",
        "//modules/map/pnc_map",
        "//modules/planning/common/trajectory:discretized_trajectory",
        "//modules/planning/common/trajectory:publishable_trajectory",
        "//modules/planning/proto:planning_config_proto",
        "//modules/planning/proto:planning_proto",
        "//modules/planning/reference_line:reference_line_provider",
    ],
)

cc_library(
    name = "frame_open_space",
    srcs = [
        "frame_open_space.cc",
    ],
    hdrs = [
        "frame_open_space.h",
    ],
    deps = [
        ":change_lane_decider",
        ":indexed_queue",
        ":lag_prediction",
        ":obstacle",
        "//modules/common:log",
        "//modules/common/adapters:adapter_manager",
        "//modules/common/configs:vehicle_config_helper",
        "//modules/common/monitor_log",
        "//modules/common/vehicle_state:vehicle_state_provider",
        "//modules/map/hdmap:hdmap_util",
        "//modules/map/pnc_map",
        "//modules/planning/common/trajectory:discretized_trajectory",
        "//modules/planning/common/trajectory:publishable_trajectory",
        "//modules/planning/proto:planning_config_proto",
        "//modules/planning/proto:planning_proto",
        "//modules/common/math:geometry",
        "@eigen",
    ],
)

cc_test(
    name = "frame_test",
    size = "small",
    srcs = [
        "frame_test.cc",
    ],
    data = [
        "//modules/planning/common:common_testdata",
    ],
    deps = [
        ":frame",
        "//modules/common/util",
        "//modules/planning/proto:planning_config_proto",
        "//modules/map/hdmap:hdmap_util",
        "@gtest//:main",
    ],
)

cc_test(
    name = "frame_open_space_test",
    size = "small",
    srcs = [
        "frame_open_space_test.cc",
    ],
    data = [
        "//modules/planning/common:common_testdata",
    ],
    deps = [
        ":frame_open_space",
        "//modules/common/proto:pnc_point_proto",
        "//modules/common/util",
        "//modules/common/vehicle_state/proto:vehicle_state_proto",
        "//modules/planning/proto:planning_config_proto",
        "@eigen",
        "@gtest//:main",
    ],
)

cc_library(
    name = "speed_limit",
    srcs = [
        "speed_limit.cc",
    ],
    hdrs = [
        "speed_limit.h",
    ],
    deps = [
        "//modules/common/math",
        "//modules/planning/proto:planning_proto",
    ],
)

cc_test(
    name = "speed_limit_test",
    size = "small",
    srcs = [
        "speed_limit_test.cc",
    ],
    deps = [
        ":speed_limit",
        "@gtest//:main",
    ],
)

cc_library(
    name = "ego_info",
    srcs = [
        "ego_info.cc",
    ],
    hdrs = [
        "ego_info.h",
    ],
    deps = [
        ":obstacle",
        "//modules/common:log",
        "//modules/common/configs:vehicle_config_helper",
        "//modules/common/configs/proto:vehicle_config_proto",
        "//modules/common/math:geometry",
        "//modules/common/vehicle_state/proto:vehicle_state_proto",
        "//modules/planning/reference_line",
        "@eigen",
    ],
)

cc_test(
    name = "ego_info_test",
    size = "small",
    srcs = [
        "ego_info_test.cc",
    ],
    deps = [
        ":ego_info",
        ":frame",
        "@gtest//:main",
    ],
)

cc_library(
    name = "planning_common",
    deps = [
        ":ego_info",
        ":frame",
        ":frame_open_space",
        ":planning_gflags",
        ":speed_limit",
        "//modules/common:log",
        "//modules/localization/common:localization_common",
        "//modules/localization/proto:localization_proto",
        "@eigen",
    ],
)

cpplint()
