# gazelle v0.44.0 go package name

After `gazelle` `v0.44.0`, protobuf files with option like below

```protobuf
option go_package = "github.com/fardream/gazelleprotoname;foo";
```

and `gazelle` directives

```starlark
# gazelle:proto package
# gazelle:proto_group go_package
```

will generate `proto_library` and `go_proto_library` with name `gazelleprotoname_foo_proto` and `gazelleprotoname_foo_go_proto`, instead of `foo_proto` and `foo_go_proto`.
this looks like to be introduced in PR: [Replace illegal characters in underscores when deriving proto RuleName](https://github.com/bazel-contrib/bazel-gazelle/pull/2105).

reverting gazelle back to `v0.43.0` will produce correct rules

```
diff --git a/BUILD.bazel b/BUILD.bazel
index a4227e6..d6c57ad 100644
--- a/BUILD.bazel
+++ b/BUILD.bazel
@@ -8,14 +8,14 @@ load("@rules_proto//proto:defs.bzl", "proto_library")
 gazelle(name = "gazelle")

 proto_library(
-    name = "gazelleprotoname_foo_proto",
+    name = "foo_proto",
     srcs = ["foo.proto"],
     visibility = ["//visibility:public"],
 )

 go_proto_library(
-    name = "gazelleprotoname_foo_go_proto",
+    name = "foo_go_proto",
     importpath = "github.com/fardream/gazelleprotoname",
-    proto = ":gazelleprotoname_foo_proto",
+    proto = ":foo_proto",
     visibility = ["//visibility:public"],
 )
diff --git a/MODULE.bazel b/MODULE.bazel
index 57347ef..5994804 100644
--- a/MODULE.bazel
+++ b/MODULE.bazel
@@ -3,7 +3,7 @@
 bazel_dep(name = "protobuf", version = "31.1")
 bazel_dep(name = "rules_go", version = "0.55.1")
 bazel_dep(name = "rules_proto", version = "7.1.0")
-bazel_dep(name = "gazelle", version = "0.44.0")
+bazel_dep(name = "gazelle", version = "0.43.0")

 go_sdk = use_extension("@rules_go//go:extensions.bzl", "go_sdk")
 go_sdk.download(version = "1.24.4")
```
