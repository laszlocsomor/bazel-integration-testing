workspace(name = "build_bazel_integration_testing")

load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

# Remote execution infra
# Required configuration for remote build execution
bazel_toolchains_version="be10bee3010494721f08a0fccd7f57411a1e773e"
bazel_toolchains_sha256="5962fe677a43226c409316fcb321d668fc4b7fa97cb1f9ef45e7dc2676097b26"
http_archive(
         name = "bazel_toolchains",
         urls = [
           "https://mirror.bazel.build/github.com/bazelbuild/bazel-toolchains/archive/%s.tar.gz"%bazel_toolchains_version,
           "https://github.com/bazelbuild/bazel-toolchains/archive/%s.tar.gz"%bazel_toolchains_version
         ],
         strip_prefix = "bazel-toolchains-%s"%bazel_toolchains_version,
         sha256 = bazel_toolchains_sha256,
)

## Sanity checks

git_repository(
    name = "bazel_skylib",
    remote = "https://github.com/bazelbuild/bazel-skylib",
    tag = "0.8.0",
)

load("@bazel_skylib//lib:versions.bzl", "versions")

versions.check("0.6.0")

## Linting

load("//private:format.bzl", "format_repositories")

format_repositories()

#### Fetch remote resources

## Python

http_archive(
    name = "com_google_python_gflags",
    build_file_content = """
py_library(
    name = "gflags",
    srcs = [
        "gflags.py",
        "gflags_validators.py",
    ],
    visibility = ["//visibility:public"],
)
""",
    sha256 = "344990e63d49b9b7a829aec37d5981d558fea12879f673ee7d25d2a109eb30ce",
    strip_prefix = "python-gflags-python-gflags-2.0",
    url = "https://github.com/google/python-gflags/archive/python-gflags-2.0.zip",
)

## Java

maven_jar(
    name = "com_google_truth",
    artifact = "com.google.truth:truth:jar:0.31",
)

maven_jar(
    name = "com_spotify_hamcrest_optional",
    artifact = "com.spotify:hamcrest-optional:jar:1.1.1",
)

## golang

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "io_bazel_rules_go",
    sha256 = "86ae934bd4c43b99893fc64be9d9fc684b81461581df7ea8fc291c816f5ee8c5",
    urls = [
        "https://github.com/bazelbuild/rules_go/releases/download/0.18.3/rules_go-0.18.3.tar.gz",
    ],
)

http_archive(
    name = "bazel_gazelle",
    sha256 = "3c681998538231a2d24d0c07ed5a7658cb72bfb5fd4bf9911157c0e9ac6a2687",
    urls = [
        "https://github.com/bazelbuild/bazel-gazelle/releases/download/0.17.0/bazel-gazelle-0.17.0.tar.gz",
    ],
)

load("@io_bazel_rules_go//go:deps.bzl", "go_rules_dependencies", "go_register_toolchains")

go_rules_dependencies()

go_register_toolchains()

load("@bazel_gazelle//:deps.bzl", "gazelle_dependencies")

gazelle_dependencies()

#### Use remote resources

## java

load("//:bazel_integration_test.bzl", "bazel_java_integration_test_deps")

bazel_java_integration_test_deps()

load("//tools:import.bzl", "bazel_external_dependency_archive")

bazel_external_dependency_archive(
    name = "test_archive",
    srcs = {
        "90a8e1603eeca48e7e879f3afbc9560715322985f39a274f6f6070b43f9d06fe": [
            "http://repo1.maven.org/maven2/junit/junit/4.11/junit-4.11.jar",
            "http://maven.ibiblio.org/maven2/junit/junit/4.11/junit-4.11.jar",
        ],
        "e0de160b129b2414087e01fe845609cd55caec6820cfd4d0c90fabcc7bdb8c1e": [
            "http://repo1.maven.org/maven2/com/beust/jcommander/1.72/jcommander-1.72.jar",
            "http://maven.ibiblio.org/maven2/com/beust/jcommander/1.72/jcommander-1.72.jar",
        ],
    },
)

bazel_external_dependency_archive(
    name = "test_archive2",
    srcs = {
        "91c77044a50c481636c32d916fd89c9118a72195390452c81065080f957de7ff": [
            "http://repo1.maven.org/maven2/javax/inject/javax.inject/1/javax.inject-1.jar",
            "http://maven.ibiblio.org/maven2/javax/inject/javax.inject/1/javax.inject-1.jar",
        ],
    },
)

bazel_external_dependency_archive(
   name = "bazel_toolchains_test",
   srcs = {
       "5962fe677a43226c409316fcb321d668fc4b7fa97cb1f9ef45e7dc2676097b26": [
           "https://github.com/bazelbuild/bazel-toolchains/archive/be10bee3010494721f08a0fccd7f57411a1e773e.tar.gz",
       ],
   }
)

## Your new language here!
