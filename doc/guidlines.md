# Connector Guidelines

- Keep Connector generic: it owns socket transport of opaque buffers, not files.
- Do not introduce file paths, NFS handles, metadata records, hashes, profiler
  records, or product scenario logic.
- Preserve zero-copy ownership between queues where possible. Socket writes may
  use scatter/gather, and socket reads should write directly into preallocated
  destination buffers.
- Keep transport jobs composable as regular Piper jobs connected by queues.
- Any transport optimization must be benchmarked without changing payload
  semantics.
