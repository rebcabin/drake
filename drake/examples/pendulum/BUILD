# -*- python -*-
# This file contains rules for Bazel; see drake/doc/bazel.rst.

load(
    "//tools:drake.bzl",
    "drake_cc_googletest",
    "drake_cc_library",
    "drake_cc_binary",
)
load("//tools:install_data.bzl", "install_data")
load("//tools:lint.bzl", "add_lint_tests")

drake_cc_library(
    name = "pendulum_state_vector",
    srcs = ["gen/pendulum_state_vector.cc"],
    hdrs = ["gen/pendulum_state_vector.h"],
    deps = [
        "//drake/systems/framework:vector",
    ],
)

drake_cc_library(
    name = "pendulum_plant",
    srcs = ["pendulum_plant.cc"],
    hdrs = ["pendulum_plant.h"],
    deps = [
        ":pendulum_state_vector",
        "//drake/systems/framework",
    ],
)

drake_cc_binary(
    name = "pendulum_run_dynamics",
    srcs = ["pendulum_run_dynamics.cc"],
    add_test_rule = 1,
    data = [":models"],
    deps = [
        ":pendulum_plant",
        "//drake/common:find_resource",
        "//drake/lcm",
        "//drake/multibody:rigid_body_tree",
        "//drake/multibody/joints",
        "//drake/multibody/parsers",
        "//drake/multibody/rigid_body_plant:drake_visualizer",
        "//drake/systems/analysis:simulator",
        "//drake/systems/framework:diagram",
        "//drake/systems/primitives:constant_vector_source",
    ],
)

drake_cc_binary(
    name = "pendulum_run_energy_shaping",
    srcs = ["pendulum_run_energy_shaping.cc"],
    add_test_rule = 1,
    data = [":models"],
    deps = [
        ":pendulum_plant",
        "//drake/common:find_resource",
        "//drake/lcm",
        "//drake/multibody/joints",
        "//drake/multibody/parsers",
        "//drake/multibody/rigid_body_plant:drake_visualizer",
        "//drake/systems/analysis:simulator",
        "//drake/systems/framework:diagram",
        "//drake/systems/framework:leaf_system",
    ],
)

drake_cc_binary(
    name = "pendulum_run_lqr",
    srcs = ["pendulum_run_lqr.cc"],
    add_test_rule = 1,
    data = [":models"],
    deps = [
        ":pendulum_plant",
        "//drake/common:find_resource",
        "//drake/lcm",
        "//drake/multibody/joints",
        "//drake/multibody/parsers",
        "//drake/multibody/rigid_body_plant:drake_visualizer",
        "//drake/systems/analysis:simulator",
        "//drake/systems/controllers:linear_quadratic_regulator",
        "//drake/systems/framework:diagram",
        "//drake/systems/framework:leaf_system",
    ],
)

drake_cc_binary(
    name = "pendulum_run_swing_up",
    srcs = ["pendulum_run_swing_up.cc"],
    add_test_rule = 1,
    data = [":models"],
    test_rule_args = ["--target_realtime_rate=0.0"],
    deps = [
        "pendulum_plant",
        "//drake/common:find_resource",
        "//drake/common:is_approx_equal_abstol",
        "//drake/lcm",
        "//drake/multibody/joints",
        "//drake/multibody/parsers",
        "//drake/multibody/rigid_body_plant:drake_visualizer",
        "//drake/systems/analysis:simulator",
        "//drake/systems/controllers:pid_controlled_system",
        "//drake/systems/framework",
        "//drake/systems/primitives:trajectory_source",
        "//drake/systems/trajectory_optimization:direct_collocation",
        "@com_github_gflags_gflags//:gflags",
    ],
)

drake_cc_binary(
    name = "print_symbolic_dynamics",
    srcs = ["print_symbolic_dynamics.cc"],
    add_test_rule = 1,
    deps = [
        ":pendulum_plant",
        "//drake/common:symbolic",
    ],
)

# === test/ ===

drake_cc_googletest(
    name = "pendulum_urdf_dynamics_test",
    data = ["Pendulum.urdf"],
    deps = [
        ":pendulum_plant",
        "//drake/common:eigen_matrix_compare",
        "//drake/common:find_resource",
        "//drake/multibody:rigid_body_tree",
        "//drake/multibody/parsers",
        "//drake/multibody/rigid_body_plant",
    ],
)

drake_cc_googletest(
    name = "pendulum_plant_test",
    deps = [
        ":pendulum_plant",
        "//drake/common:autodiff",
    ],
)

install_data()

add_lint_tests()
