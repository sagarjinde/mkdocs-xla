# **Kernel**

A data-parallel kernel (code entity) for launching via the [StreamExecutor](StreamExecutor.md).

### Attributes Overview

```cpp
  // The StreamExecutor that loads this kernel object.
  StreamExecutor *parent_;

  // Implementation delegated to for platform-specific functionality.
  std::unique_ptr<internal::KernelInterface> implementation_;
```