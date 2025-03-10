load("//xla:xla.bzl", "xla_cc_test")
load("@tsl//tsl:tsl.default.bzl", "get_compatible_with_cloud")
load("@tsl//tsl/platform:build_config.bzl", "if_llvm_system_z_available", "tf_platform_deps")
load("@tsl//tsl/platform:rules_cc.bzl", "cc_library")

package(
    # copybara:uncomment default_applicable_licenses = ["//tensorflow:license"],
    default_visibility = [
        "//xla:internal",
        "@tf_runtime//:friends",
    ],
    licenses = ["notice"],
)

cc_library(
    name = "arguments",
    srcs = ["arguments.cc"],
    hdrs = ["arguments.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        ":async_runtime",
        ":errors",
        ":types",
        "//xla:shape_util",
        "//xla/mlir/runtime/transforms:type_converter",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/types:span",
        "@llvm-project//llvm:Support",
    ],
)

xla_cc_test(
    name = "arguments_test",
    srcs = ["arguments_test.cc"],
    deps = [
        ":arguments",
        ":types",
        "@llvm-project//llvm:Support",
        "@tsl//tsl/platform:test",
        "@tsl//tsl/platform:test_benchmark",
        "@tsl//tsl/platform:test_main",
    ],
)

cc_library(
    name = "async_runtime",
    srcs = ["async_runtime.cc"],
    hdrs = ["async_runtime.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        "@com_google_absl//absl/base:dynamic_annotations",
        "@com_google_absl//absl/status",
        "@tf_runtime//:async_value",
        "@tf_runtime//:ref_count",
        "@tsl//tsl/platform:env",
        "@tsl//tsl/platform:platform_port",
    ],
)

xla_cc_test(
    name = "async_runtime_test",
    srcs = ["async_runtime_test.cc"],
    deps = [
        ":async_runtime",
        "@com_google_absl//absl/status",
        "@tf_runtime//:async_value",
        "@tsl//tsl/platform:test",
        "@tsl//tsl/platform:test_main",
    ],
)

cc_library(
    name = "async_values_cache",
    hdrs = ["async_values_cache.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        "@tsl//tsl/platform",
    ] + tf_platform_deps(
        "async_values_cache",
        platform_dir = "//xla/runtime/",
    ),
)

cc_library(
    name = "constraints",
    srcs = ["constraints.cc"],
    hdrs = ["constraints.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        ":errors",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@llvm-project//llvm:Support",
    ],
)

cc_library(
    name = "custom_call",
    srcs = ["custom_call.cc"],
    hdrs = ["custom_call.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        ":async_runtime",
        ":diagnostics",
        ":errors",
        ":logical_result",
        ":map_by_type",
        ":memref_view",
        ":state",
        ":type_id",
        "//xla:shape_util",
        "//xla/runtime/ffi:ffi_abi",
        "@com_google_absl//absl/base:dynamic_annotations",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/types:span",
        "@eigen_archive//:eigen3",
        "@llvm-project//llvm:Support",
        "@tf_runtime//:async_value",
    ],
)

xla_cc_test(
    name = "custom_call_test",
    srcs = ["custom_call_test.cc"],
    deps = [
        ":arguments",
        ":async_runtime",
        ":custom_call",
        ":custom_call_registry",
        ":diagnostics",
        ":executable",
        ":jit_executable",
        ":module",
        ":state",
        "//xla/mlir/runtime/ir/tests:testlib",
        "//xla/mlir/runtime/transforms:compilation_pipeline_gpu",
        "//xla/mlir/runtime/transforms:custom_call_encoding",
        "@tsl//tsl/platform:test",
        "@tsl//tsl/platform:test_benchmark",
        "@tsl//tsl/platform:test_main",
    ],
)

cc_library(
    name = "custom_call_registry",
    srcs = ["custom_call_registry.cc"],
    hdrs = ["custom_call_registry.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        ":custom_call",
        "@llvm-project//llvm:Support",
    ],
)

cc_library(
    name = "diagnostics",
    srcs = ["diagnostics.cc"],
    hdrs = ["diagnostics.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        ":logical_result",
        "@com_google_absl//absl/status",
        "@llvm-project//llvm:Support",
        "@tsl//tsl/platform:logging",
    ],
)

xla_cc_test(
    name = "diagnostics_test",
    srcs = ["diagnostics_test.cc"],
    deps = [
        ":diagnostics",
        "@com_google_absl//absl/status",
        "@tsl//tsl/platform:test",
        "@tsl//tsl/platform:test_main",
    ],
)

cc_library(
    name = "errors",
    hdrs = ["errors.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/strings:str_format",
        "@llvm-project//llvm:Support",
    ],
)

cc_library(
    name = "executable",
    srcs = ["executable.cc"],
    hdrs = ["executable.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        ":arguments",
        ":async_runtime",
        ":custom_call",
        ":custom_call_registry",
        ":diagnostics",
        ":errors",
        ":execution_engine",
        ":logical_result",
        ":memory_mapper",
        ":results",
        ":runtime",
        ":type_id",
        ":types",
        "//xla/mlir/runtime/utils:async_runtime_api",
        "//xla/mlir/runtime/utils:c_runner_utils",
        "//xla/mlir/runtime/utils:float_16bits",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings:str_format",
        "@llvm-project//llvm:OrcJIT",
        "@llvm-project//llvm:Support",
    ],
)

xla_cc_test(
    name = "executable_test",
    srcs = ["executable_test.cc"],
    tags = ["nomsan"],  # TODO(ezhulenev): Find msan error in LLVM coroutine passes
    deps = [
        ":arguments",
        ":async_runtime",
        ":custom_call_registry",
        ":jit_executable",
        ":logical_result",
        ":results",
        ":types",
        "//xla/mlir/runtime/transforms:compilation_pipeline_options",
        "//xla/mlir/runtime/transforms/tests:testlib_pipeline",
        "//xla/mlir/runtime/utils:async_runtime_api",
        "@com_google_absl//absl/base:dynamic_annotations",
        "@llvm-project//mlir:Support",
        "@tsl//tsl/platform:test",
        "@tsl//tsl/platform:test_benchmark",
        "@tsl//tsl/platform:test_main",
    ],
)

cc_library(
    name = "execution_engine",
    srcs = ["execution_engine.cc"],
    hdrs = ["execution_engine.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        ":errors",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings:str_format",
        "@llvm-project//llvm:Core",
        "@llvm-project//llvm:ExecutionEngine",
        "@llvm-project//llvm:OrcJIT",
        "@llvm-project//llvm:Support",
        "@llvm-project//llvm:TransformUtils",
        "@llvm-project//llvm:X86AsmParser",
        "@llvm-project//llvm:X86CodeGen",
    ] + select({
        "@tsl//tsl:arm_any": [
            "@llvm-project//llvm:AArch64AsmParser",  # fixdeps: keep
            "@llvm-project//llvm:AArch64CodeGen",  # fixdeps: keep
        ],
        "@tsl//tsl:linux_ppc64le": [
            "@llvm-project//llvm:PowerPCAsmParser",  # fixdeps: keep
            "@llvm-project//llvm:PowerPCCodeGen",  # fixdeps: keep
        ],
        "@tsl//tsl:macos_arm64": [
            "@llvm-project//llvm:AArch64AsmParser",  # fixdeps: keep
            "@llvm-project//llvm:AArch64CodeGen",  # fixdeps: keep
        ],
        "//conditions:default": [
        ],
    }) + if_llvm_system_z_available([
        "@llvm-project//llvm:SystemZAsmParser",  # fixdeps: keep
        "@llvm-project//llvm:SystemZCodeGen",  # fixdeps: keep
    ]),
)

cc_library(
    name = "ffi",
    srcs = ["ffi.cc"],
    hdrs = ["ffi.h"],
    deps = [
        ":custom_call",
        ":module",
        "//xla/runtime/ffi:ffi_c_api_hdrs",
        "@com_google_absl//absl/algorithm:container",
        "@com_google_absl//absl/status",
    ],
)

xla_cc_test(
    name = "ffi_test",
    srcs = ["ffi_test.cc"],
    deps = [
        ":arguments",
        ":async_runtime",
        ":custom_call",
        ":custom_call_registry",
        ":diagnostics",
        ":executable",
        ":ffi",
        ":jit_executable",
        ":results",
        "//xla/mlir/runtime/transforms:compilation_pipeline_gpu",
        "//xla/runtime/ffi:ffi_api",
        "//xla/runtime/ffi:ffi_c_api_hdrs",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/strings:str_format",
        "@tsl//tsl/platform:test",
        "@tsl//tsl/platform:test_benchmark",
        "@tsl//tsl/platform:test_main",
    ],
)

cc_library(
    name = "jit_executable",
    srcs = ["jit_executable.cc"],
    hdrs = ["jit_executable.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        ":arguments",
        ":async_values_cache",
        ":constraints",
        ":errors",
        "//xla/mlir/runtime/transforms:jit_compiler",
        "//xla/mlir/runtime/utils:constraints",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@llvm-project//llvm:Support",
        "@tf_runtime//:async_value",
    ],
)

cc_library(
    name = "logical_result",
    hdrs = ["logical_result.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = ["@llvm-project//mlir:Support"],
)

cc_library(
    name = "map_by_type",
    hdrs = ["map_by_type.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        ":type_id",
        "@llvm-project//llvm:Support",
    ],
)

xla_cc_test(
    name = "map_by_type_test",
    srcs = ["map_by_type_test.cc"],
    deps = [
        ":map_by_type",
        "@tsl//tsl/platform:test",
        "@tsl//tsl/platform:test_benchmark",
        "@tsl//tsl/platform:test_main",
    ],
)

cc_library(
    name = "memory_mapper",
    srcs = ["memory_mapper.cc"],
    hdrs = ["memory_mapper.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        "@llvm-project//llvm:ExecutionEngine",
        "@llvm-project//llvm:Support",
        "@tsl//tsl/platform",
    ] + tf_platform_deps(
        "memory_mapper",
        platform_dir = "//xla/runtime/",
    ),
)

cc_library(
    name = "memref_view",
    hdrs = ["memref_view.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        "//xla:xla_data_proto_cc",
        "@com_google_absl//absl/types:span",
    ],
)

cc_library(
    name = "module",
    hdrs = ["module.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        ":custom_call",
        ":custom_call_registry",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
    ],
)

cc_library(
    name = "module_registry",
    srcs = ["module_registry.cc"],
    hdrs = ["module_registry.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        ":module",
    ],
)

xla_cc_test(
    name = "module_test",
    srcs = ["module_test.cc"],
    deps = [
        ":custom_call",
        ":module",
        "@tsl//tsl/platform:test",
        "@tsl//tsl/platform:test_main",
    ],
)

cc_library(
    name = "results",
    hdrs = ["results.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        ":logical_result",
        ":types",
    ],
)

xla_cc_test(
    name = "results_test",
    srcs = ["results_test.cc"],
    deps = [
        ":logical_result",
        ":results",
        ":types",
        "@tsl//tsl/platform:test",
        "@tsl//tsl/platform:test_benchmark",
        "@tsl//tsl/platform:test_main",
    ],
)

cc_library(
    name = "runtime",
    hdrs = ["runtime.h"],
    compatible_with = get_compatible_with_cloud(),
)

cc_library(
    name = "state",
    hdrs = ["state.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/synchronization",
    ],
)

xla_cc_test(
    name = "state_test",
    srcs = ["state_test.cc"],
    deps = [
        ":state",
        "@tsl//tsl/platform:test",
        "@tsl//tsl/platform:test_benchmark",
        "@tsl//tsl/platform:test_main",
    ],
)

cc_library(
    name = "symbolic_shape",
    srcs = ["symbolic_shape.cc"],
    hdrs = ["symbolic_shape.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        ":arguments",
        ":constraints",
        ":logical_result",
        ":types",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@llvm-project//llvm:Support",
    ],
)

xla_cc_test(
    name = "symbolic_shape_test",
    srcs = ["symbolic_shape_test.cc"],
    deps = [
        ":arguments",
        ":constraints",
        ":symbolic_shape",
        ":types",
        "@llvm-project//llvm:Support",
        "@tsl//tsl/platform:test",
        "@tsl//tsl/platform:test_benchmark",
        "@tsl//tsl/platform:test_main",
    ],
)

cc_library(
    name = "types",
    srcs = ["types.cc"],
    hdrs = ["types.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        "//xla:shape_util",
        "//xla:xla_data_proto_cc",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@llvm-project//llvm:Support",
    ],
)

cc_library(
    name = "tracing",
    hdrs = ["tracing.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        ":custom_call",
        ":type_id",
    ],
)

cc_library(
    name = "type_id",
    srcs = ["type_id.cc"],
    hdrs = ["type_id.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        "@com_google_absl//absl/container:flat_hash_map",
        "@llvm-project//mlir:Support",
    ],
)

cc_library(
    name = "compiler",
    hdrs = ["compiler.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        "@llvm-project//llvm:Support",
    ],
)

cc_library(
    name = "cpu_event",
    hdrs = ["cpu_event.h"],
)

xla_cc_test(
    name = "type_id_test",
    srcs = ["type_id_test.cc"],
    deps = [
        ":type_id",
        "@tsl//tsl/platform:test",
        "@tsl//tsl/platform:test_benchmark",
        "@tsl//tsl/platform:test_main",
    ],
)
