load("//tsl/platform:rules_cc.bzl", "cc_library")
load("//tsl:tsl.bzl", "set_external_visibility")
load("//tsl/platform:build_config.bzl", "tsl_cc_test")

package(
    # copybara:uncomment default_applicable_licenses = ["//tensorflow:license"],
    default_visibility = set_external_visibility([
        "//tsl:internal",
    ]),
    licenses = ["notice"],
)

cc_library(
    name = "persistent_cache_repository",
    hdrs = ["persistent_cache_repository.h"],
    deps = [
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
    ],
)

cc_library(
    name = "fake_cache_repository",
    testonly = True,
    hdrs = ["fake_cache_repository.h"],
    deps = [
        ":persistent_cache_repository",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/synchronization",
    ],
)

cc_library(
    name = "mock_cache_repository",
    testonly = True,
    hdrs = ["mock_cache_repository.h"],
    deps = [
        ":persistent_cache_repository",
        "//tsl/platform:test",
    ],
)

tsl_cc_test(
    name = "fake_cache_repository_test",
    size = "small",
    srcs = ["fake_cache_repository_test.cc"],
    deps = [
        ":fake_cache_repository",
        "//tsl/lib/core:status_test_util",
        "//tsl/platform:status_matchers",
        "//tsl/platform:test",
        "//tsl/platform:test_main",
    ],
)
