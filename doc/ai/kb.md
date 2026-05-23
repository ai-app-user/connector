# Connector AI Knowledge Base

- Repository purpose: generic TCP/Unix socket transport for opaque buffers.
- Project boundary: depends on Piper primitives; does not depend on Filer or
  Hypersync product code.
- Source-root order in workspace builds:
  `piper/src -> connector/src -> filer/src -> hypersync/src`.
- Namespace is still `hypersync` during the low-risk extraction phase.
