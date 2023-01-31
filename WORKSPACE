workspace(name = "workerd")

# ========================================================================================
# Bazel basics

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive", "http_file")
load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository", "new_git_repository")

http_archive(
    name = "bazel_skylib",
    sha256 = "f24ab666394232f834f74d19e2ff142b0af17466ea0c69a3f4c276ee75f6efce",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/bazel-skylib/releases/download/1.4.0/bazel-skylib-1.4.0.tar.gz",
        "https://github.com/bazelbuild/bazel-skylib/releases/download/1.4.0/bazel-skylib-1.4.0.tar.gz",
    ],
)

load("@bazel_skylib//:workspace.bzl", "bazel_skylib_workspace")

bazel_skylib_workspace()

http_archive(
    name = "rules_foreign_cc",
    sha256 = "2a4d07cd64b0719b39a7c12218a3e507672b82a97b98c6a89d38565894cf7c51",
    strip_prefix = "rules_foreign_cc-0.9.0",
    url = "https://github.com/bazelbuild/rules_foreign_cc/archive/0.9.0.tar.gz",
)

load("@rules_foreign_cc//foreign_cc:repositories.bzl", "rules_foreign_cc_dependencies")

rules_foreign_cc_dependencies()

# ========================================================================================
# Simple dependencies

http_archive(
    name = "capnp-cpp",
    sha256 = "0a863f8319ed697f9649b1ee7e89c51430651e99079bd9c3d549a61d1ecddc93",
    strip_prefix = "capnproto-capnproto-f557d9d/c++",
    type = "tgz",
    urls = ["https://github.com/capnproto/capnproto/tarball/f557d9d728a773d954cf0586fe6e78adf828c5f4"],
)

http_archive(
    name = "ssl",
    sha256 = "501c9eeac199e7e964b30c99af074d9eb95132cd8bcb22447a082eb745a51a1a",
    strip_prefix = "google-boringssl-ed2e74e",
    type = "tgz",
    # from master-with-bazel branch
    urls = ["https://github.com/google/boringssl/tarball/ed2e74e737dc802ed9baad1af62c1514430a70d6"],
)

http_archive(
    name = "com_cloudflare_lol_html",
    build_file = "//:build/BUILD.lol-html",
    sha256 = "8f53ebce850292cf80150de3391e771e5f757ece2bdf4a819c263be64edb7c4f",
    strip_prefix = "cloudflare-lol-html-1b64a2e",
    type = "tgz",
    url = "https://github.com/cloudflare/lol-html/tarball/1b64a2ed0d719ce5dfac316108ca2dfad73ff9b4",
)

# ========================================================================================
# tcmalloc

# tcmalloc requires Abseil.
#
# WARNING: This MUST appear before rules_fuzzing_depnedencies(), below. Otherwise,
#   rules_fuzzing_depnedencies() will choose to pull in a different version of Abseil that is too
#   old for tcmalloc. Absurdly, Bazel simply ignores later attempts to define the same repo name,
#   rather than erroring out. Thus this leads to confusing compiler errors in tcmalloc complaining
#   that ABSL_ATTRIBUTE_PURE_FUNCTION is not defined.
http_archive(
    name = "com_google_absl",
    sha256 = "d5c91248c33269fcc7ab35897315a45cfa2c37abb4c6d4ed36cb5c82f366367a",
    strip_prefix = "abseil-cpp-b3162b1da62711c663d0025e2eabeb83fd1f2728",
    urls = ["https://github.com/abseil/abseil-cpp/archive/b3162b1da62711c663d0025e2eabeb83fd1f2728.zip"],
)

# tcmalloc requires this "rules_fuzzing" package. Its build files fail analysis without it, even
# though it is unused for our purposes.
http_archive(
    name = "rules_fuzzing",
    sha256 = "d9002dd3cd6437017f08593124fdd1b13b3473c7b929ceb0e60d317cb9346118",
    strip_prefix = "rules_fuzzing-0.3.2",
    urls = ["https://github.com/bazelbuild/rules_fuzzing/archive/v0.3.2.zip"],
)

load("@rules_fuzzing//fuzzing:repositories.bzl", "rules_fuzzing_dependencies")

rules_fuzzing_dependencies()

load("@rules_fuzzing//fuzzing:init.bzl", "rules_fuzzing_init")

rules_fuzzing_init()

# OK, now we can bring in tcmalloc itself.
http_archive(
    name = "com_google_tcmalloc",
    sha256 = "5a051c876bd49ee4aeffaed60a5934046fd89ce9e86b687693e8d6c5fe7c210c",
    strip_prefix = "google-tcmalloc-ca82471",
    type = "tgz",
    url = "https://github.com/google/tcmalloc/tarball/ca82471188f4832e82d2e77078ecad66f4c425d5",
)

# ========================================================================================
# Rust bootstrap
#
# workerd uses some Rust libraries, especially lolhtml for implementing HtmlRewriter.

http_file(
    name = "cargo_bazel_linux_x64",
    executable = True,
    sha256 = "7a592329c1f411687cad19b9f24018c5c08498494f463be36ff13ee6834eff15",
    urls = [
        "https://github.com/bazelbuild/rules_rust/releases/download/0.17.0/cargo-bazel-x86_64-unknown-linux-gnu",
    ],
)

http_file(
    name = "cargo_bazel_linux_arm64",
    executable = True,
    sha256 = "c4df000f36e479daa15e9e31c4f65483aa92fb7d317d82cc5a77dff976c1a73a",
    urls = [
        "https://github.com/bazelbuild/rules_rust/releases/download/0.17.0/cargo-bazel-aarch64-unknown-linux-gnu",
    ],
)

http_file(
    name = "cargo_bazel_macos_x64",
    executable = True,
    sha256 = "8f0bfa72544f664b155cb1f3dc9583895c979fe39546c31597f4f25a8b821f62",
    urls = [
        "https://github.com/bazelbuild/rules_rust/releases/download/0.17.0/cargo-bazel-x86_64-apple-darwin",
    ],
)

http_file(
    name = "cargo_bazel_macos_arm64",
    executable = True,
    sha256 = "c439f582ca4ae0ddd03791a2f6c3dc838000e0638891fc6506e217adca7cfc4d",
    urls = [
        "https://github.com/bazelbuild/rules_rust/releases/download/0.17.0/cargo-bazel-aarch64-apple-darwin",
    ],
)

http_archive(
    name = "rules_rust",
    sha256 = "d125fb75432dc3b20e9b5a19347b45ec607fabe75f98c6c4ba9badaab9c193ce",
    urls = [
        "https://github.com/bazelbuild/rules_rust/releases/download/0.17.0/rules_rust-v0.17.0.tar.gz",
    ],
)

load("@rules_rust//rust:repositories.bzl", "rules_rust_dependencies", "rust_register_toolchains")

rules_rust_dependencies()

rust_register_toolchains(
    edition = "2018",
    versions = ["1.66.0"],
)

load("@rules_rust//crate_universe:repositories.bzl", "crate_universe_dependencies")

crate_universe_dependencies()

load("//rust-deps/crates:crates.bzl", "crate_repositories")

crate_repositories()

load("//rust-deps/cxxbridge_crates:crates.bzl", cxxbridge_repositories = "crate_repositories")

cxxbridge_repositories()

load("@rules_rust//tools/rust_analyzer:deps.bzl", "rust_analyzer_dependencies")

rust_analyzer_dependencies()

# ========================================================================================
# Node.js bootstrap
#
# workerd uses Node.js scripts for generating TypeScript types.

http_archive(
    name = "aspect_rules_js",
    sha256 = "9f51475dd2f99abb015939b1cf57ab5f15ef36ca6d2a67104450893fd0aa5c8b",
    strip_prefix = "rules_js-1.16.0",
    url = "https://github.com/aspect-build/rules_js/archive/refs/tags/v1.16.0.tar.gz",
)

http_archive(
    name = "aspect_rules_ts",
    sha256 = "acb20a4e41295d07441fa940c8da9fd02f8637391fd74a14300586a3ee244d59",
    strip_prefix = "rules_ts-1.2.0",
    url = "https://github.com/aspect-build/rules_ts/archive/refs/tags/v1.2.0.tar.gz",
)

load("@aspect_rules_js//js:repositories.bzl", "rules_js_dependencies")

rules_js_dependencies()

load("@rules_nodejs//nodejs:repositories.bzl", "nodejs_register_toolchains")

nodejs_register_toolchains(
    name = "nodejs",
    node_version = "18.10.0",
)

load("@aspect_rules_ts//ts:repositories.bzl", "rules_ts_dependencies", TS_LATEST_VERSION = "LATEST_VERSION")

rules_ts_dependencies(ts_version = TS_LATEST_VERSION)

load("@aspect_rules_js//npm:npm_import.bzl", "npm_translate_lock")

npm_translate_lock(
    name = "npm",
    patch_args = {
        "capnp-ts@0.7.0": ["-p1"],
    },
    # Patches required for `capnp-ts` to type-check
    patches = {
        "capnp-ts@0.7.0": ["//:patches/capnp-ts@0.7.0.patch"],
    },
    pnpm_lock = "//:pnpm-lock.yaml",
)

load("@npm//:repositories.bzl", "npm_repositories")

npm_repositories()

# ========================================================================================
# V8 and its dependencies
#
# Note that googlesource does not generate tarballs deterministically, so we cannot use
# http_archive: https://github.com/google/gitiles/issues/84
#
# It would seem that googlesource would rather we use git protocol (ideally with shallow clones).
# Fine, we can do that.
#
# Note that while there is an official mirror of V8 on GitHub, there do not appear to be
# mirrors of the dependencies: zlib (Chromium fork), icu (Chromium fork), and trace_event. So
# fetching from GitHub instead wouldn't really solve the problem.

git_repository(
    name = "v8",
    commit = "18865d6af0404f2d2aeb1c99dd73503364ce0967",
    patch_args = ["-p1"],
    patches = [
        "//:patches/v8/0001-Allow-manually-setting-ValueDeserializer-format-vers.patch",
        "//:patches/v8/0002-Allow-manually-setting-ValueSerializer-format-versio.patch",
        "//:patches/v8/0003-Make-icudata-target-public.patch",
        "//:patches/v8/0004-Add-ArrayBuffer-MaybeNew.patch",
        "//:patches/v8/0005-Revert-bazel-Add-hide-symbols-from-release-fast-buil.patch",
        "//:patches/v8/0006-bazel-Allow-compiling-on-macOS-catalina-and-ventura.patch",
    ],
    remote = "https://chromium.googlesource.com/v8/v8.git",
    shallow_since = "1662649276 +0000",
)

new_git_repository(
    name = "com_googlesource_chromium_icu",
    build_file = "@v8//:bazel/BUILD.icu",
    commit = "20f8ac695af59b6c830def7d4e95bfeb13dd7be5",
    patch_cmds = ["find source -name BUILD.bazel | xargs rm"],
    remote = "https://chromium.googlesource.com/chromium/deps/icu.git",
    shallow_since = "1660168635 +0000",
)

new_git_repository(
    name = "com_googlesource_chromium_base_trace_event_common",
    build_file = "@v8//:bazel/BUILD.trace_event_common",
    commit = "521ac34ebd795939c7e16b37d9d3ddb40e8ed556",
    remote = "https://chromium.googlesource.com/chromium/src/base/trace_event/common.git",
    shallow_since = "1659619139 -0700",
)

http_archive(
    name = "rules_python",
    sha256 = "239900c5f4df223fcfbebcefe42dbf5a376e0b99e4314b0cfab00b4ee5985b95",
    strip_prefix = "rules_python-0.17.3",
    url = "https://github.com/bazelbuild/rules_python/archive/0.17.3.tar.gz",
)

load("@rules_python//python:pip.bzl", "pip_install")

pip_install(
    name = "v8_python_deps",
    extra_pip_args = ["--require-hashes"],
    requirements = "@v8//:bazel/requirements.txt",
)

bind(
    name = "zlib_compression_utils",
    actual = "@com_googlesource_chromium_zlib//:zlib_compression_utils",
)

bind(
    name = "icu",
    actual = "@com_googlesource_chromium_icu//:icu",
)

bind(
    name = "base_trace_event_common",
    actual = "@com_googlesource_chromium_base_trace_event_common//:trace_event_common",
)

# Tell workerd code where to find v8.
#
# We indirect through `@workerd-v8` to allow dependents to override how and where `v8` is built.
#
# TODO(cleanup): There must be a better way to do this?
new_local_repository(
    name = "workerd-v8",
    build_file_content = """cc_library(
        name = "v8",
        deps = ["@v8//:v8_icu", "@workerd//:icudata-embed"],
        visibility = ["//visibility:public"])""",
    path = "empty",
)

# ========================================================================================
# Tools

# Hedron's Compile Commands Extractor for Bazel
# https://github.com/hedronvision/bazel-compile-commands-extractor
http_archive(
    name = "hedron_compile_commands",
    sha256 = "ab6c6b4ceaf12b224e571ec075fd79086c52c3430993140bb2ed585b08dfc552",
    strip_prefix = "bazel-compile-commands-extractor-d1e95ec162e050b04d0a191826f9bc478de639f7",
    url = "https://github.com/hedronvision/bazel-compile-commands-extractor/archive/d1e95ec162e050b04d0a191826f9bc478de639f7.tar.gz",
)

load("@hedron_compile_commands//:workspace_setup.bzl", "hedron_compile_commands_setup")

hedron_compile_commands_setup()
