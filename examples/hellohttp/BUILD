# Copyright 2017 The Bazel Authors. All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
package(default_visibility = ["//visibility:public"])

licenses(["notice"])  # Apache 2.0

load("@k8s_deploy//:defaults.bzl", "k8s_deploy")

k8s_deploy(
    name = "staging",
    template = "deployment.yaml",
)

load("@io_bazel_rules_jsonnet//jsonnet:jsonnet.bzl", "jsonnet_to_json")

jsonnet_to_json(
    name = "deployment",
    src = "deployment.jsonnet",
    outs = ["deployment.json"],
    deps = [
        "//examples:container_lib",
        "//examples:deploy_lib",
    ],
)

# Verify that deployment.json == deployment.yaml
py_test(
    name = "deployment_test",
    srcs = ["deployment_test.py"],
    data = [
        ":deployment.json",
        ":deployment.yaml",
    ],
    deps = [
        "@yaml//:yaml",
    ],
)
