# Connector

Connector owns reusable transport jobs for moving opaque buffers across TCP or
Unix sockets.

The project boundary is intentionally small:

- socket helpers and scoped socket descriptors
- buffer sender/receiver jobs
- stream buffer sender/receiver jobs
- priority buffer sender jobs
- endpoint parsing for buffer transports

Connector does not understand file metadata, file payload semantics, hashing,
NFS, profiler policy, copy/sync behavior, or product scenarios. It receives
buffers and lengths, moves bytes over sockets, and emits buffers.

Current consumers include Connector through the shared workspace build:

```text
piper/src -> utils/src -> connector/src -> filer/src -> hypersync/src
```

The C++ namespace remains `hypersync` during this extraction phase so the move
is mechanical and low-risk. Namespace cleanup can happen later after the project
boundary has settled.
