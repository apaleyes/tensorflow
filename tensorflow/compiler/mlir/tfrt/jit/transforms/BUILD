load("@llvm-project//mlir:tblgen.bzl", "gentbl_cc_library")
load("@tf_runtime//:build_defs.bzl", "tfrt_cc_library")

# TF to TFRT kernels conversion.
package(
    default_visibility = ["//tensorflow/compiler/mlir/tfrt:friends"],
    licenses = ["notice"],
)

tfrt_cc_library(
    name = "tf_cpurt_clustering",
    srcs = ["tf_cpurt_clustering.cc"],
    hdrs = ["tf_cpurt_clustering.h"],
    deps = [
        "//tensorflow/compiler/mlir/tensorflow:tensorflow_ops",
        "//tensorflow/compiler/mlir/tensorflow:tensorflow_passes",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Support",
        "@tf_runtime//backends/cpu:cpurt_support",
    ],
)

gentbl_cc_library(
    name = "tf_cpurt_passes_inc_gen",
    tbl_outs = [
        (
            [
                "-gen-pass-decls",
                "-name=TFCPURT",
            ],
            "tf_cpurt_passes.h.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "tf_cpurt_passes.td",
    deps = ["@llvm-project//mlir:PassBaseTdFiles"],
)

cc_library(
    name = "tf_cpurt_passes",
    srcs = [
        "tf_cpurt_buffer_forwarding.cc",
        "tf_cpurt_clustering_pass.cc",
        "tf_cpurt_codegen_matmul.cc",
        "tf_cpurt_codegen_reduction.cc",
        "tf_cpurt_copy_removal.cc",
        "tf_cpurt_fission.cc",
        "tf_cpurt_matmul_specialization.cc",
        "tf_cpurt_pad_tiled_ops.cc",
        "tf_cpurt_passes.cc",
        "tf_cpurt_peel_tiled_loops.cc",
        "tf_cpurt_symbolic_shape_optimization.cc",
        "tf_cpurt_vectorize_tiled_ops.cc",
    ],
    hdrs = ["tf_cpurt_passes.h"],
    deps = [
        ":tf_cpurt_clustering",
        ":tf_cpurt_passes_inc_gen",
        "//tensorflow/compiler/mlir/hlo",
        "//tensorflow/compiler/mlir/hlo:all_passes",
        "//tensorflow/compiler/mlir/hlo:legalize_to_linalg",
        "//tensorflow/compiler/mlir/tensorflow",
        "//tensorflow/compiler/mlir/tensorflow:tensorflow_ops",
        "//tensorflow/compiler/mlir/tensorflow:tensorflow_passes",
        "//tensorflow/compiler/mlir/tensorflow:tensorflow_types",
        "//tensorflow/compiler/mlir/tools/kernel_gen/transforms:passes",
        "//tensorflow/compiler/mlir/xla:xla_legalize_tf",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:Async",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:LinalgOps",
        "@llvm-project//mlir:LinalgTransforms",
        "@llvm-project//mlir:MemRefDialect",
        "@llvm-project//mlir:MemRefTransforms",
        "@llvm-project//mlir:Pass",
        "@llvm-project//mlir:SCFDialect",
        "@llvm-project//mlir:ShapeToStandard",
        "@llvm-project//mlir:ShapeTransforms",
        "@llvm-project//mlir:StandardOps",
        "@llvm-project//mlir:StandardOpsTransforms",
        "@llvm-project//mlir:Support",
        "@llvm-project//mlir:TensorDialect",
        "@llvm-project//mlir:TensorTransforms",
        "@llvm-project//mlir:Transforms",
        "@llvm-project//mlir:VectorOps",
    ],
    alwayslink = 1,
)

gentbl_cc_library(
    name = "tf_cpurt_test_passes_inc_gen",
    tbl_outs = [
        (
            [
                "-gen-pass-decls",
                "-name=TFCPURTTest",
            ],
            "tf_cpurt_test_passes.h.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "tf_cpurt_test_passes.td",
    deps = ["@llvm-project//mlir:PassBaseTdFiles"],
)

cc_library(
    name = "tf_cpurt_test_passes",
    srcs = ["tf_cpurt_test_passes.cc"],
    hdrs = ["tf_cpurt_test_passes.h"],
    deps = [
        ":tf_cpurt_clustering",
        ":tf_cpurt_test_passes_inc_gen",
        "//tensorflow/compiler/mlir/tensorflow:tensorflow_passes",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:Pass",
    ],
    alwayslink = 1,
)
