load("@base_images_docker//package_managers:download_pkgs.bzl", "download_pkgs")
load("@base_images_docker//package_managers:install_pkgs.bzl", "install_pkgs")
load("@io_bazel_rules_docker//container:container.bzl", "container_image")
load("@io_bazel_rules_docker//contrib:test.bzl", "container_test")

download_pkgs(
    name = "java_and_python_pkgs",
    image_tar = "@ubuntu1604//image",
    packages = [
        "openjdk-8-jdk",
        "python",
        "python-dev",
        "python-pip",
        "python3",
        "python3-pip",
    ],
)

install_pkgs(
    name = "java_python_pkgs_image",
    image_tar = "@ubuntu1604//image",
    installables_tar = ":java_and_python_pkgs.tar",
    installation_cleanup_commands = "rm -rf /var/lib/apt/lists/*",
    output_image_name = "java_python_pkgs_image",
)

# Wrapper targets for the tarred images are required in order to be able to run
# commandTests in container_test targets.

container_image(
    name = "java_python_bazel_wrapper",
    base = ":java_python_pkgs_image.tar",
)

container_image(
    name = "java_python_dockerfile_wrapper",
    base = "@java_python_dockerfile//image:dockerfile_image.tar",
)

# Tests

container_test(
    name = "java_python_bazel_test",
    configs = ["//run_instruction/test_configs:java_python.yaml"],
    image = ":java_python_bazel_wrapper",
)

container_test(
    name = "java_python_dockerfile_test",
    configs = ["//run_instruction/test_configs:java_python.yaml"],
    image = ":java_python_dockerfile_wrapper",
)
