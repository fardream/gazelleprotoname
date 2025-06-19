# gazelle v0.44.0 go package name

After `gazelle` `v0.44.0`, protobuf files with option like below

```protobuf
option go_package = "github.com/fardream/gazelleprotoname;foo";
```

and `gazelle` directives

```skylark
# gazelle:proto package
# gazelle:proto_group go_package
```

will generate `proto_library` and `go_proto_library` with name `gazelleprotoname_foo_proto` and `gazelleprotoname_foo_go_proto`, instead of `foo_proto` and `foo_go_proto`.
this looks like to be introduced in PR: [Replace illegal characters in underscores when deriving proto RuleName](https://github.com/bazel-contrib/bazel-gazelle/pull/2105)
