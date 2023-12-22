# **StreamExecutor**

A StreamExecutor manages a single device, in terms of executing work (kernel
launches) and memory management (allocation/deallocation, memory copies to
and from the device). It is conceptually the "handle" for a device.

### Attributes Overview

```cpp
  // Reference to the platform that created this executor.
  const Platform* platform_;

  // Pointer to the platform-specific-interface implementation. This is
  // delegated to by the interface routines in pointer-to-implementation
  // fashion.
  std::unique_ptr<internal::StreamExecutorInterface> implementation_;
```

### Functions Overview

- Load [Kernel](Kernel.md)
- Perform device memory management

```cpp
  // Retrieves (loads) a kernel for the platform this StreamExecutor is acting
  // upon, if one exists.
  tsl::Status GetKernel(const MultiKernelLoaderSpec& spec, Kernel* kernel);

  // Loads a module for the platform this StreamExecutor is acting upon.
  tsl::Status LoadModule(const MultiModuleLoaderSpec& spec,
                         ModuleHandle* module_handle);

  // Synchronously allocates an array on the device of type T with element_count
  // elements.
  template <typename T>
  DeviceMemory<T> AllocateArray(uint64_t element_count,
                                int64_t memory_space = 0);

  // Warning: use Stream::ThenLaunch instead, this method is not for general
  // consumption. However, this is the only way to launch a kernel for which
  // the type signature is only known at runtime; say, if an application
  // supports loading/launching kernels with arbitrary type signatures.
  // In this case, the application is expected to know how to do parameter
  // packing that obeys the contract of the underlying platform implementation.
  //
  // Launches a data parallel kernel with the given thread/block
  // dimensionality and already-packed args/sizes to pass to the underlying
  // platform driver.
  //
  // This is called by Stream::Launch() to delegate to the platform's launch
  // implementation in StreamExecutorInterface::Launch().
  tsl::Status Launch(Stream* stream, const ThreadDim& thread_dims,
                     const BlockDim& block_dims, const Kernel& kernel,
                     const KernelArgs& args);

  // Submits command buffer for execution to the underlying platform driver.
  tsl::Status Submit(Stream* stream, const CommandBuffer& command_buffer);
```