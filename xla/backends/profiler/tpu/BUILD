load("@tsl//tsl/platform:rules_cc.bzl", "cc_library")
load("@tsl//tsl/profiler/builds:build_config.bzl", "tf_profiler_copts")
load("@tsl//tsl:tsl.bzl", "if_with_tpu_support")

package(
    # copybara:uncomment default_applicable_licenses = ["//tensorflow:license"],
    licenses = ["notice"],
)

cc_library(
    name = "tpu_tracer",
    srcs = if_with_tpu_support(["tpu_tracer.cc"]),
    copts = tf_profiler_copts(),
    visibility = [
        "//third_party/xprof/plugin/tensorboard_plugin_profile/integration_tests/tpu/tensorflow:__pkg__",
        "//xla:internal",
    ],
    deps = [
        "//xla/stream_executor/tpu:status_helper",
        "//xla/stream_executor/tpu:tpu_api",
        "//xla/stream_executor/tpu:tpu_ops_c_api_hdrs",
        "@com_google_absl//absl/strings",
        "@tsl//tsl/platform:errors",
        "@tsl//tsl/platform:status",
        "@tsl//tsl/platform:types",
        "@tsl//tsl/profiler/lib:profiler_factory",
        "@tsl//tsl/profiler/lib:profiler_interface",
        "@tsl//tsl/profiler/protobuf:profiler_options_proto_cc",
        "@tsl//tsl/profiler/protobuf:xplane_proto_cc",
        "@tsl//tsl/profiler/utils:xplane_schema",
    ],
    alwayslink = True,
)
