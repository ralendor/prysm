"""
cargo-raze crate build file.

DO NOT EDIT! Replaced on runs of cargo-raze
"""
package(default_visibility = [
  # Public for visibility by "@raze__crate__version//" targets.
  #
  # Prefer access through "//cargo", which limits external
  # visibility to explicit Cargo.toml dependencies.
  "//visibility:public",
])

licenses([
  "notice", # "MIT"
])

load(
    "@io_bazel_rules_rust//rust:rust.bzl",
    "rust_library",
    "rust_binary",
    "rust_test",
)

rust_binary(
    name = "crunchy_build_script",
    srcs = glob(["**/*.rs"]),
    crate_root = "build.rs",
    edition = "2015",
    deps = [
    ],
    rustc_flags = [
        "--cap-lints=allow",
    ],
    crate_features = [
      "default",
      "limit_128",
    ],
    data = glob(["*"]),
    version = "0.1.6",
    visibility = ["//visibility:private"],
)

genrule(
    name = "crunchy_build_script_executor",
    srcs = glob(["*", "**/*.rs"]),
    outs = ["crunchy_out_dir_outputs.tar.gz"],
    tools = [
      ":crunchy_build_script",
    ],
    local = 1,
    cmd = "mkdir -p crunchy_out_dir_outputs/;"
        + " (export CARGO_MANIFEST_DIR=\"$$PWD/$$(dirname $(location :Cargo.toml))\";"
        # TODO(acmcarther): This needs to be revisited as part of the cross compilation story.
        #                   See also: https://github.com/google/cargo-raze/pull/54
        + " export TARGET='x86_64-unknown-linux-gnu';"
        + " export RUST_BACKTRACE=1;"
        + " export CARGO_FEATURE_DEFAULT=1;"
        + " export CARGO_FEATURE_LIMIT_128=1;"
        + " export OUT_DIR=$$PWD/crunchy_out_dir_outputs;"
        + " export BINARY_PATH=\"$$PWD/$(location :crunchy_build_script)\";"
        + " export OUT_TAR=$$PWD/$@;"
        + " cd $$(dirname $(location :Cargo.toml)) && $$BINARY_PATH && tar -czf $$OUT_TAR -C $$OUT_DIR .)"
)


rust_library(
    name = "crunchy",
    crate_root = "src/lib.rs",
    crate_type = "lib",
    edition = "2015",
    srcs = glob(["**/*.rs"]),
    deps = [
    ],
    rustc_flags = [
        "--cap-lints=allow",
    ],
    out_dir_tar = ":crunchy_build_script_executor",
    version = "0.1.6",
    crate_features = [
        "default",
        "limit_128",
    ],
)

