# Connector Design

Connector is the generic socket transport layer for WSync projects.

## Boundary

Connector is allowed to depend on Piper primitives such as `RawBufferPool`,
`BufQueue`, and `ThreadedJob`. It must not depend on Filer or Hypersync product
code.

Connector jobs treat payloads as opaque byte buffers:

```text
[BufferSender] -> socket -> [BufferReceiver]
```

The wire frame contains only enough information to move an owned buffer between
processes:

- frame magic and version
- source pool identifier
- payload length
- raw payload bytes

Any interpretation of those payload bytes belongs downstream.

## Ownership Rules

- Sender jobs consume `BufferHandle` values from `BufQueue`, write the selected
  payload length, and release the source handle.
- Receiver jobs acquire destination buffers from local pools, read socket bytes
  directly into those buffers, and push handles into output queues.
- Connector must not copy file metadata into custom records or inspect file
  payload layouts.
- Connector benchmark and null/discard modes may optimize socket ingestion, but
  those modes still operate only on buffers and lengths.

## Project Order

Workspace builds include Connector after Piper and before filesystem/product
layers:

```text
piper/src -> connector/src -> filer/src -> hypersync/src
```
