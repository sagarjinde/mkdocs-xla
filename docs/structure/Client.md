# **Client**

XLA service's client object -- wraps the service with convenience and
lifetime-oriented methods.

### Attributes Overview

- Interface to [Service](Service.md)

```cpp
  ServiceInterface* stub_;  // Stub that this client is connected on.
```

### Functions Overview

- Request to Compile and Execute [XlaComputation](XlaComputation.md) on [Service](Service.md)
- Transfer data to and from [Service](Service.md)

```cpp
  // Compile the computation with the given argument shapes and returns the
  // handle to the compiled executable. The compiled executable is cached on the
  // service, and the returned handle can be used for execution without
  // re-compile.
  StatusOr<ExecutionHandle> Compile(
      const XlaComputation& computation,
      absl::Span<const Shape> argument_shapes,
      const ExecutionOptions* execution_options = nullptr);

  // Executes the compiled executable for the given handle with the given
  // arguments and returns the global data that was produced from the execution.
  StatusOr<std::unique_ptr<GlobalData>> Execute(
      const ExecutionHandle& handle, absl::Span<GlobalData* const> arguments,
      ExecutionProfile* execution_profile = nullptr);

  // Transfer the global data provided to this client process, which is
  // returned in the provided literal.
  StatusOr<Literal> Transfer(const GlobalData& data,
                             const Shape* shape_with_layout = nullptr);

  // Transfer the given literal to the server. This allocates memory on the
  // device and copies the literal's contents over. Returns a global data handle
  // that can be used to refer to this value from the client.
  StatusOr<std::unique_ptr<GlobalData>> TransferToServer(
      const LiteralSlice& literal, const DeviceHandle* device_handle = nullptr);
```

!!! note
    - Literal is data on host
    - GlobalData is data on device