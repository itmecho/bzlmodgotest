# Bazel modules grpc-gateway issue repro

This repo is a broken state with bazel `8.1.1` when trying to compile `grpc-gateway` proto go code.

The issue seems to be that `grpc-gateway` can't resolve `@googleapis`.

This is the current output.

```
❯ bazel --version
bazel 8.1.1

# This command succeeds so I think googleapis is set up correctly
❯ bazel build @googleapis//google/api:annotations_go_proto
INFO: Invocation ID: 16768fca-21da-4c34-bed1-5f9d37ae8eb2
INFO: Analyzed target @@googleapis+//google/api:annotations_go_proto (75 packages loaded, 879 targets configured).
INFO: From Linking external/protobuf+/src/google/protobuf/io/libio_win32.a [for tool]:
warning: /Library/Developer/CommandLineTools/usr/bin/libtool: archive library: bazel-out/darwin_arm64-opt-exec-ST-d57f47055a04/bin/external/protobuf+/src/google/protobuf/io/libio_win32.a the table of contents is empty (no object file members in the library define global symbols)
INFO: From Linking external/protobuf+/protoc [for tool]:
ld: warning: ignoring duplicate libraries: '-lm', '-lpthread'
INFO: Found 1 target...
Target @@googleapis+//google/api:annotations_go_proto up-to-date:
  bazel-bin/external/googleapis+/google/api/annotations_go_proto.a
INFO: Elapsed time: 62.263s, Critical Path: 25.05s
INFO: 967 processes: 87 remote cache hit, 421 internal, 459 darwin-sandbox.
INFO: Build completed successfully, 967 total actions

# This command fails with an error around grpc-gateway
❯ bazel build //proto:proto_go_proto
INFO: Invocation ID: 8c172c36-b0a4-41f5-b5e9-d5259cb75c39
ERROR: no such package '@@[unknown repo 'googleapis' requested from @@gazelle++go_deps+com_github_grpc_ecosystem_grpc_gateway_v2]//google/api': The repository '@@[unknown repo 'googleapis' requested from @@gazelle++go_deps+com_github_grpc_ecosystem_grpc_gateway_v2]' could not be resolved: No repository visible as '@googleapis' from repository '@@gazelle++go_deps+com_github_grpc_ecosystem_grpc_gateway_v2'
ERROR: /private/var/tmp/_bazel_<USER>/fc7c0518f4c97ba6af31cd3db54a67ca/external/gazelle++go_deps+com_github_grpc_ecosystem_grpc_gateway_v2/internal/descriptor/apiconfig/BUILD.bazel:7:14: no such package '@@[unknown repo 'googleapis' requested from @@gazelle++go_deps+com_github_grpc_ecosystem_grpc_gateway_v2]//google/api': The repository '@@[unknown repo 'googleapis' requested from @@gazelle++go_deps+com_github_grpc_ecosystem_grpc_gateway_v2]' could not be resolved: No repository visible as '@googleapis' from repository '@@gazelle++go_deps+com_github_grpc_ecosystem_grpc_gateway_v2' and referenced by '@@gazelle++go_deps+com_github_grpc_ecosystem_grpc_gateway_v2//internal/descriptor/apiconfig:apiconfig_proto'
ERROR: Analysis of target '//proto:proto_go_proto' failed; build aborted: Analysis failed
INFO: Elapsed time: 0.156s, Critical Path: 0.00s
INFO: 1 process: 1 internal.
ERROR: Build did NOT complete successfully
FAILED:
    Fetching repository @@gazelle++go_deps+org_golang_google_grpc; starting
    Fetching repository @@gazelle++go_deps+org_golang_google_protobuf; starting
    Fetching repository @@gazelle++go_deps+org_golang_x_net; starting
    Fetching repository @@gazelle++go_deps+org_golang_google_genproto_googleapis_api; starting
    Fetching repository @@abseil-cpp+; starting
    Fetching repository @@gazelle++go_deps+org_golang_x_text; starting
    Fetching repository @@gazelle++go_deps+in_gopkg_yaml_v3; starting
```
