package(
    default_visibility = ["//src/test/java/com/google/devtools/build/lib:__subpackages__"],
)

filegroup(
    name = "srcs",
    srcs = glob(["**"]) + [
        "//src/test/java/com/google/devtools/build/lib/skylark:srcs",
        "//src/test/java/com/google/devtools/build/lib/skyframe:srcs",
        "//src/test/java/com/google/devtools/build/lib/rules/repository:srcs",
        "//src/test/java/com/google/devtools/build/lib/bazel/repository:srcs",
        "//src/test/java/com/google/devtools/build/lib/buildtool:srcs",
    ],
    visibility = ["//src:__pkg__"],
)

# This should correspond to the list of "EMBEDDED_TOOLS" in TestConstants.java.bazel.
filegroup(
    name = "embedded_scripts",
    testonly = 1,
    data = [
        "//src/main/tools:build-runfiles",
        "//src/main/tools:build_interface_so",
        "//src/main/tools:namespace-sandbox",
        "//src/main/tools:process-wrapper",
    ],
)

java_library(
    name = "testutil",
    srcs = glob(["testutil/*.java"]),
    visibility = ["//visibility:public"],
    deps = [
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:clock",
        "//src/main/java/com/google/devtools/build/lib:collect",
        "//src/main/java/com/google/devtools/build/lib:concurrent",
        "//src/main/java/com/google/devtools/build/lib:events",
        "//src/main/java/com/google/devtools/build/lib:io",
        "//src/main/java/com/google/devtools/build/lib:os_util",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/actions",
        "//src/main/protobuf:invocation_policy_java_proto",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_library(
    name = "foundations_testutil",
    srcs = glob([
        "vfs/util/*.java",
        "events/util/*.java",
    ]),
    data = select({
        "//src:windows": [],
        "//conditions:default": [
            "//src/main/native:libunix.dylib",
            "//src/main/native:libunix.so",
        ],
    }),
    deps = [
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:clock",
        "//src/main/java/com/google/devtools/build/lib:concurrent",
        "//src/main/java/com/google/devtools/build/lib:events",
        "//src/main/java/com/google/devtools/build/lib:io",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//src/main/java/com/google/devtools/build/lib:shell",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_library(
    name = "test_runner",
    srcs = ["AllTests.java"],
    deps = [
        ":testutil",
        "//third_party:junit4",
    ],
)

java_test(
    name = "foundations_test",
    srcs = glob(
        [
            "cmdline/*.java",
            "concurrent/*.java",
            "collect/*.java",
            "collect/nestedset/*.java",
            "events/*.java",
            "testutil/*Test.java",
            "unix/*.java",
            "util/*.java",
            "util/io/*.java",
            "vfs/*.java",
            "vfs/inmemoryfs/*.java",
        ],
        # java_rules_skylark doesn't support resource loading with
        # qualified paths.
        exclude = [
            "util/DependencySetWindowsTest.java",
            "util/ResourceFileLoaderTest.java",
            "vfs/PathFragmentWindowsTest.java",
            "vfs/PathWindowsTest.java",
        ],
    ),
    data = glob(["vfs/*.zip"]),
    tags = ["foundations"],
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":foundations_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:base-util",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:clock",
        "//src/main/java/com/google/devtools/build/lib:collect",
        "//src/main/java/com/google/devtools/build/lib:concurrent",
        "//src/main/java/com/google/devtools/build/lib:events",
        "//src/main/java/com/google/devtools/build/lib:io",
        "//src/main/java/com/google/devtools/build/lib:os_util",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//src/main/java/com/google/devtools/build/lib:shell",
        "//src/main/java/com/google/devtools/build/lib:unix",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/common/options",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

# Tests that test Windows-specific functionality that run on other operating
# systems
java_test(
    name = "windows_test",
    srcs = [
        "util/DependencySetWindowsTest.java",
        "vfs/PathFragmentWindowsTest.java",
        "vfs/PathWindowsTest.java",
    ],
    jvm_flags = ["-Dblaze.os=Windows"],
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":foundations_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:clock",
        "//src/main/java/com/google/devtools/build/lib:inmemoryfs",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib:windows",
        "//src/main/java/com/google/devtools/common/options",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

# Tests that need to run on Windows
java_test(
    name = "windows-tests",
    srcs = glob(
        ["windows/*.java"],
        exclude = ["windows/MockSubprocess.java"],
    ),
    data = [
        ":MockSubprocess_deploy.jar",
    ] + select({
        "//src:windows": ["//src/main/native:windows_jni.dll"],
        "//conditions:default": [
            "//src/main/native:libunix.dylib",
            "//src/main/native:libunix.so",
        ],
    }),
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:os_util",
        "//src/main/java/com/google/devtools/build/lib:windows",
        "//third_party:truth",
    ],
)

java_library(
    name = "actions_testutil",
    srcs = glob([
        "actions/util/*.java",
    ]),
    deps = [
        ":foundations_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:clock",
        "//src/main/java/com/google/devtools/build/lib:concurrent",
        "//src/main/java/com/google/devtools/build/lib:events",
        "//src/main/java/com/google/devtools/build/lib:io",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/actions",
        "//src/main/java/com/google/devtools/build/skyframe",
        "//src/main/java/com/google/devtools/common/options",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:mockito",
        "//third_party:truth",
    ],
)

java_test(
    name = "actions_test",
    srcs = glob([
        "actions/*.java",
    ]),
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":actions_testutil",
        ":foundations_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:clock",
        "//src/main/java/com/google/devtools/build/lib:collect",
        "//src/main/java/com/google/devtools/build/lib:concurrent",
        "//src/main/java/com/google/devtools/build/lib:events",
        "//src/main/java/com/google/devtools/build/lib:java-rules",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/actions",
        "//src/main/java/com/google/devtools/build/lib/rules/cpp",
        "//src/main/java/com/google/devtools/common/options",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:mockito",
        "//third_party:truth",
    ],
)

java_library(
    name = "analysis_testutil",
    srcs = glob([
        "analysis/mock/BazelAnalysisMock.java",
        "analysis/util/*.java",
        "exec/util/*.java",
    ]),
    resources = [
        "MOCK_CROSSTOOL",
    ],
    tags = ["analysis"],
    exports = [
        "//src/test/java/com/google/devtools/build/skyframe:testutil",
    ],
    deps = [
        ":actions_testutil",
        ":foundations_testutil",
        ":packages_testutil",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:android-rules",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:clock",
        "//src/main/java/com/google/devtools/build/lib:collect",
        "//src/main/java/com/google/devtools/build/lib:concurrent",
        "//src/main/java/com/google/devtools/build/lib:events",
        "//src/main/java/com/google/devtools/build/lib:flags",
        "//src/main/java/com/google/devtools/build/lib:io",
        "//src/main/java/com/google/devtools/build/lib:java-rules",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//src/main/java/com/google/devtools/build/lib:python-rules",
        "//src/main/java/com/google/devtools/build/lib:runtime",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/actions",
        "//src/main/java/com/google/devtools/build/lib/query2",
        "//src/main/java/com/google/devtools/build/lib/query2:query-output",
        "//src/main/java/com/google/devtools/build/lib/rules/apple",
        "//src/main/java/com/google/devtools/build/lib/rules/cpp",
        "//src/main/java/com/google/devtools/build/lib/rules/genquery",
        "//src/main/java/com/google/devtools/build/lib/rules/objc",
        "//src/main/java/com/google/devtools/build/skyframe",
        "//src/main/java/com/google/devtools/common/options",
        "//src/main/protobuf:extra_actions_base_java_proto",
        "//src/test/java/com/google/devtools/build/lib/skyframe:testutil",
        "//src/test/java/com/google/devtools/build/skyframe:testutil",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:mockito",
        "//third_party:truth",
    ],
)

java_test(
    name = "analysis_actions_test",
    srcs = glob([
        "analysis/actions/*.java",
    ]),
    tags = ["analysis"],
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":actions_testutil",
        ":analysis_testutil",
        ":foundations_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:io",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/actions",
        "//src/main/protobuf:extra_actions_base_java_proto",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:mockito",
        "//third_party:truth",
    ],
)

java_test(
    name = "analysis_config_test",
    srcs = glob([
        "analysis/config/*.java",
    ]),
    tags = ["analysis"],
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":actions_testutil",
        ":analysis_testutil",
        ":foundations_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:java-rules",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//src/main/java/com/google/devtools/build/lib:python-rules",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/rules/cpp",
        "//src/main/java/com/google/devtools/build/lib/rules/objc",
        "//src/main/java/com/google/devtools/common/options",
        "//src/main/protobuf:extra_actions_base_java_proto",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:mockito",
        "//third_party:truth",
    ],
)

java_test(
    name = "analysis_constraints_test",
    srcs = glob([
        "analysis/constraints/*.java",
    ]),
    tags = ["analysis"],
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":actions_testutil",
        ":analysis_testutil",
        ":foundations_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//third_party:guava",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_test(
    name = "analysis_select_test",
    srcs = glob([
        "analysis/select/*.java",
    ]),
    tags = ["analysis"],
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":actions_testutil",
        ":analysis_testutil",
        ":foundations_testutil",
        ":packages_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//third_party:guava",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_test(
    name = "analysis_test",
    srcs = glob([
        "analysis/*.java",
    ]),
    shard_count = 2,
    tags = ["analysis"],
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":actions_testutil",
        ":analysis_testutil",
        ":foundations_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:collect",
        "//src/main/java/com/google/devtools/build/lib:events",
        "//src/main/java/com/google/devtools/build/lib:java-rules",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//src/main/java/com/google/devtools/build/lib:skylarkinterface",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/actions",
        "//src/main/java/com/google/devtools/build/lib/rules/cpp",
        "//src/main/java/com/google/devtools/build/skyframe",
        "//third_party:auto_value",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:mockito",
        "//third_party:truth",
    ],
)

java_test(
    name = "graph_test",
    srcs = glob([
        "graph/*.java",
    ]),
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":foundations_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:cmdline",
        "//src/main/java/com/google/devtools/build/lib:collect",
        "//src/main/java/com/google/devtools/build/lib:events",
        "//src/main/java/com/google/devtools/build/lib:graph",
        "//src/main/java/com/google/devtools/build/lib:packages-internal",
        "//third_party:guava",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_library(
    name = "packages_testutil",
    srcs = glob([
        "packages/util/*.java",
    ]),
    deps = [
        ":foundations_testutil",
        ":testutil",
        "//src/main/java/com/google/devtools/build/docgen:docgen_javalib",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:clock",
        "//src/main/java/com/google/devtools/build/lib:events",
        "//src/main/java/com/google/devtools/build/lib:flags",
        "//src/main/java/com/google/devtools/build/lib:io",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//src/main/java/com/google/devtools/build/lib:runtime",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/actions",
        "//src/main/java/com/google/devtools/build/skyframe",
        "//src/main/java/com/google/devtools/common/options",
        "//src/main/protobuf:build_java_proto",
        "//src/main/protobuf:crosstool_config_java_proto",
        "//src/main/protobuf:extra_actions_base_java_proto",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:mockito",
        "//third_party:truth",
        "//third_party/protobuf",
    ],
)

java_test(
    name = "packages_test",
    srcs = glob(
        [
            "packages/*.java",
        ],
        exclude = [
            "packages/BazelDocumentationTest.java",
        ],
    ),
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":actions_testutil",
        ":analysis_testutil",
        ":foundations_testutil",
        ":packages_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:events",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//src/main/java/com/google/devtools/build/lib:skylarkinterface",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/skyframe",
        "//src/main/protobuf:build_java_proto",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:truth",
        "//third_party/protobuf",
    ],
)

java_test(
    name = "pkgcache_test",
    srcs = glob(
        [
            "pkgcache/*.java",
        ],
    ),
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":analysis_testutil",
        ":packages_testutil",
        ":test_runner",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:flags",
        "//src/main/java/com/google/devtools/build/lib:io",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//src/main/java/com/google/devtools/build/skyframe",
        "//src/test/java/com/google/devtools/build/lib:testutil",
        "//third_party:guava",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_test(
    name = "ideinfo_test",
    srcs = glob([
        "ideinfo/*.java",
    ]),
    resources = ["ideinfo/intellij_info.bzl"],
    tags = ["ideinfo"],
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":analysis_testutil",
        ":foundations_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:collect",
        "//src/main/java/com/google/devtools/build/lib:events",
        "//src/main/java/com/google/devtools/build/lib:ideinfo",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/actions",
        "//src/main/protobuf:android_studio_ide_info_java_proto",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:mockito",
        "//third_party:truth",
        "//third_party/protobuf",
    ],
)

cc_binary(
    name = "shell/killmyself",
    srcs = ["shell/killmyself.cc"],
)

java_test(
    name = "shell_test",
    srcs = glob([
        "shell/*.java",
    ]),
    data = [
        ":shell/killmyself",
    ],
    flaky = True,
    tags = ["shell"],
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":foundations_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:collect",
        "//src/main/java/com/google/devtools/build/lib:shell",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:mockito",
        "//third_party:truth",
    ],
)

java_test(
    name = "server_test",
    srcs = glob([
        "server/*.java",
    ]),
    tags = ["server"],
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":foundations_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:collect",
        "//src/main/java/com/google/devtools/build/lib:io",
        "//src/main/java/com/google/devtools/build/lib:runtime",
        "//src/main/java/com/google/devtools/build/lib:unix",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:mockito",
        "//third_party:truth",
    ],
)

java_binary(
    name = "skylarkshell",
    srcs = ["syntax/SkylarkShell.java"],
    main_class = "com.google.devtools.build.lib.syntax.SkylarkShell",
    deps = [
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:events",
        "//src/main/java/com/google/devtools/build/lib:packages",
    ],
)

java_test(
    name = "syntax_test",
    srcs = glob([
        "syntax/*.java",
    ]),
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":foundations_testutil",
        ":syntax_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:collect",
        "//src/main/java/com/google/devtools/build/lib:concurrent",
        "//src/main/java/com/google/devtools/build/lib:events",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//src/main/java/com/google/devtools/build/lib:skylarkinterface",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/actions",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_test(
    name = "profiler-tests",
    srcs = glob([
        "profiler/*.java",
    ]),
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":foundations_testutil",
        ":syntax_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:clock",
        "//src/main/java/com/google/devtools/build/lib:profiler-output",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_test(
    name = "BazelDocumentationTests",
    size = "medium",
    srcs = ["packages/BazelDocumentationTest.java"],
    data = [
        "//site:docs/bazel-user-manual.html",
    ],
    test_class = "com.google.devtools.build.lib.packages.BazelDocumentationTest",
    deps = [
        ":foundations_testutil",
        ":packages_testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_library(
    name = "syntax_testutil",
    srcs = glob([
        "syntax/util/*.java",
    ]),
    visibility = ["//visibility:public"],
    deps = [
        ":foundations_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:collect",
        "//src/main/java/com/google/devtools/build/lib:concurrent",
        "//src/main/java/com/google/devtools/build/lib:events",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/actions",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_test(
    name = "filegroup-rules-tests",
    srcs = glob(["rules/filegroup/*.java"]),
    tags = ["rules"],
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":actions_testutil",
        ":analysis_testutil",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:java-rules",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_test(
    name = "java-rules-tests",
    srcs = glob(["rules/java/*.java"]),
    tags = ["rules"],
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":actions_testutil",
        ":analysis_testutil",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:java-rules",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_test(
    name = "cpp-rules-tests",
    srcs = glob(["rules/cpp/*.java"]),
    tags = ["rules"],
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":actions_testutil",
        ":analysis_testutil",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:cmdline",
        "//src/main/java/com/google/devtools/build/lib:flags",
        "//src/main/java/com/google/devtools/build/lib:packages-internal",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/actions",
        "//src/main/java/com/google/devtools/build/lib/rules/cpp",
        "//src/main/java/com/google/devtools/common/options",
        "//src/main/protobuf:crosstool_config_java_proto",
        "//src/main/protobuf:extra_actions_base_java_proto",
        "//src/test/java/com/google/devtools/build/lib:packages_testutil",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:jsr305",
        "//third_party:junit4",
        "//third_party:truth",
        "//third_party/protobuf",
    ],
)

java_test(
    name = "objc-rules-tests",
    srcs = glob(["rules/objc/XcodeProviderTest.java"]),
    tags = ["rules"],
    test_class = "com.google.devtools.build.lib.rules.objc.XcodeProviderTest",
    deps = [
        "//src/main/java/com/google/devtools/build/lib:cmdline",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/rules/objc",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_test(
    name = "bazel-rules-tests",
    srcs = glob([
        "bazel/rules/**/*.java",
    ]),
    resources = glob(["bazel/rules/**/*.txt"]),
    tags = ["rules"],
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":analysis_testutil",
        ":foundations_testutil",
        ":test_runner",
        "//src/main/java/com/google/devtools/build/lib:bazel-main",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:events",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/actions",
        "//src/main/protobuf:crosstool_config_java_proto",
        "//third_party:guava",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_test(
    name = "runtime-tests",
    srcs = glob(["runtime/*.java"]),
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":foundations_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:clock",
        "//src/main/java/com/google/devtools/build/lib:flags",
        "//src/main/java/com/google/devtools/build/lib:io",
        "//src/main/java/com/google/devtools/build/lib:packages",
        "//src/main/java/com/google/devtools/build/lib:runtime",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/actions",
        "//src/main/java/com/google/devtools/common/options",
        "//src/main/protobuf:invocation_policy_java_proto",
        "//src/main/protobuf:test_status_java_proto",
        "//third_party:guava",
        "//third_party:junit4",
        "//third_party:mockito",
        "//third_party:truth",
    ],
)

java_test(
    name = "sandbox-tests",
    srcs = glob(["sandbox/*.java"]),
    data = [":embedded_scripts"],
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":actions_testutil",
        ":analysis_testutil",
        ":foundations_testutil",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:clock",
        "//src/main/java/com/google/devtools/build/lib:events",
        "//src/main/java/com/google/devtools/build/lib:io",
        "//src/main/java/com/google/devtools/build/lib:os_util",
        "//src/main/java/com/google/devtools/build/lib:shell",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/actions",
        "//src/main/java/com/google/devtools/build/lib/sandbox",
        "//src/main/java/com/google/devtools/common/options",
        "//third_party:guava",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_test(
    name = "standalone-tests",
    srcs = glob(["standalone/*.java"]),
    data = [":embedded_scripts"],
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":actions_testutil",
        ":analysis_testutil",
        ":foundations_testutil",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:clock",
        "//src/main/java/com/google/devtools/build/lib:events",
        "//src/main/java/com/google/devtools/build/lib:io",
        "//src/main/java/com/google/devtools/build/lib:os_util",
        "//src/main/java/com/google/devtools/build/lib:shell",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/actions",
        "//src/main/java/com/google/devtools/build/lib/rules/apple",
        "//src/main/java/com/google/devtools/build/lib/standalone",
        "//src/main/java/com/google/devtools/common/options",
        "//third_party:guava",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_test(
    name = "exec-tests",
    srcs = glob(["exec/*.java"]),
    tags = ["exec"],
    test_class = "com.google.devtools.build.lib.AllTests",
    deps = [
        ":actions_testutil",
        ":analysis_testutil",
        ":foundations_testutil",
        ":test_runner",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:bazel-rules",
        "//src/main/java/com/google/devtools/build/lib:build-base",
        "//src/main/java/com/google/devtools/build/lib:inmemoryfs",
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib:vfs",
        "//src/main/java/com/google/devtools/build/lib/actions",
        "//third_party:guava",
        "//third_party:junit4",
        "//third_party:truth",
        "//third_party/protobuf",
    ],
)

java_binary(
    name = "MockSubprocess",
    srcs = ["windows/MockSubprocess.java"],
)

java_library(
    name = "ExampleWorker-lib",
    srcs = glob(["worker/ExampleWorker*.java"]),
    visibility = [
        "//src/test/shell/bazel:__pkg__",
    ],
    deps = [
        "//src/main/java/com/google/devtools/common/options",
        "//src/main/protobuf:worker_protocol_java_proto",
        "//third_party:guava",
    ],
)

java_binary(
    name = "ExampleWorker",
    main_class = "com.google.devtools.build.lib.worker.ExampleWorker",
    visibility = [
        "//src/test/shell/bazel:__pkg__",
    ],
    runtime_deps = [
        ":ExampleWorker-lib",
    ],
)

TEST_SUITES = [
    "ziputils",
    "rules",
    "analysis",
    "foundations",
    "shell",
    "server",
    "skyframe",
    "ideinfo",
    "exec",
]

[
    test_suite(
        name = n,
        tags = [n],
    )
    for n in TEST_SUITES
]

test_suite(
    name = "others",
    tags = ["-" + n for n in TEST_SUITES],
)
