load("//tensorflow:tensorflow.bzl", "tf_cc_binary")
load("//tensorflow/core/platform:build_config.bzl", "tf_additional_all_protos", "tf_proto_library")

package(
    default_visibility = ["//tensorflow:internal"],
    licenses = ["notice"],  # Apache 2.0
)

package_group(
    name = "friends",
    includes = ["//tensorflow:internal"],
)

package_group(
    name = "internal",
    packages = [
        "//tensorflow/core/profiler/...",
        "//tensorflow/python/eager/...",
        "//tensorflow/python/profiler/...",
        "//tensorflow/python/tpu/profiler/...",
    ],
)

tf_proto_library(
    name = "profiler_service_monitor_result_proto",
    srcs = ["profiler_service_monitor_result.proto"],
    cc_api_version = 2,
    visibility = ["//visibility:public"],
)

tf_proto_library(
    name = "profiler_service_proto",
    srcs = ["profiler_service.proto"],
    has_services = 1,
    cc_api_version = 2,
    cc_grpc_version = 1,
    protodeps = [
        ":profiler_service_monitor_result_proto",
    ],
    use_grpc_namespace = True,
    visibility = ["//visibility:public"],
)

tf_proto_library(
    name = "profiler_analysis_proto",
    srcs = ["profiler_analysis.proto"],
    has_services = 1,
    cc_api_version = 2,
    cc_grpc_version = 1,
    protodeps = [":profiler_service_proto"],
    use_grpc_namespace = True,
    visibility = ["//visibility:public"],
)

tf_proto_library(
    name = "protos_all",
    srcs = [
        "profile.proto",
        "tfprof_log.proto",
        "tfprof_options.proto",
        "tfprof_output.proto",
    ],
    cc_api_version = 2,
    protodeps = tf_additional_all_protos(),
    visibility = [":friends"],
)

cc_library(
    name = "profiler_impl",
    visibility = ["//tensorflow:__pkg__"],
    deps = [
        "//tensorflow/core/profiler/internal:annotation_stack_impl",
        "//tensorflow/core/profiler/internal:profiler_factory_impl",
        "//tensorflow/core/profiler/internal:traceme_recorder_impl",
        "//tensorflow/core/profiler/lib:profiler_session_impl",
    ],
    alwayslink = True,
)

# Libraries and binaries of TensorFlow profiler 1.x.
tf_cc_binary(
    name = "profiler",
    srcs = ["profiler.cc"],
    visibility = ["//visibility:public"],
    deps = [
        ":protos_all_cc",
        "//tensorflow/c:c_api",
        "//tensorflow/c:checkpoint_reader",
        "//tensorflow/core:framework_headers_lib",
        "//tensorflow/core:framework_internal",
        "//tensorflow/core:lib",
        "//tensorflow/core:protos_all_cc",
        "//tensorflow/core/profiler:tfprof_options",
        "//tensorflow/core/profiler/internal:tfprof_stats",
        "//tensorflow/core/profiler/internal:tfprof_utils",
        "//tensorflow/core/profiler/internal/advisor:tfprof_advisor",
        "@linenoise",
    ],
)

cc_library(
    name = "tfprof_options",
    srcs = ["tfprof_options.cc"],
    hdrs = ["tfprof_options.h"],
    visibility = ["//visibility:public"],
    deps = [
        ":protos_all_cc",
        "//tensorflow/core:framework_headers_lib",
        "//tensorflow/core:lib",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
    ],
)

filegroup(
    name = "mobile_srcs",
    srcs = [
        "//tensorflow/core/profiler/internal:mobile_srcs",
        "//tensorflow/core/profiler/lib:mobile_srcs",
    ],
    visibility = ["//tensorflow/core:__pkg__"],
)
