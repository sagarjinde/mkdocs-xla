# **Service**

The XLA service object, which is the same across all platforms. It maintains
the service state of computations and allocations, and delegates
target-specific requests to the target-specific infrastructure
(target-specific compiler, StreamExecutor).

### Attributes Overview

- Cache build [Executable](Executable.md)s
- Tracks the stage of execution

```cpp
  ServiceOptions options_;

  // Cache containing previously built Executables.
  CompilationCache compilation_cache_;

  // Tracks asynchronously launched executions via the API.
  ExecutionTracker execution_tracker_;
```

### Functions Overview

- Compiles and Executes [XlaComputation](XlaComputation.md)
- Build [StreamExecutor](StreamExecutor.md) and [Executable](Executable.md)
- Transfer data to and from [Clinet](Client.md)

```cpp
  // Compiles a computation into an executable. The request contains the whole
  // computation graph. Returns the handle to the executable.
  Status Compile(const CompileRequest* arg, CompileResponse* result) override;

  // Executes an executable with the provided global data passes as immutable
  // arguments. The request contains the handle to the executable. Returns
  // global data output and execution timing.
  Status Execute(const ExecuteRequest* arg, ExecuteResponse* result) override;

  // Requests that global data be transferred to the client in literal form.
  Status TransferToClient(const TransferToClientRequest* arg,
                          TransferToClientResponse* result) override;

  // Transfers data from a literal provided by the client, into device memory.
  Status TransferToServer(const TransferToServerRequest* arg,
                          TransferToServerResponse* result) override;

  // Prepare the executors for executing parallel.
  StatusOr<std::vector<se::StreamExecutor*>> GetExecutors(
      const ExecutionOptions& execution_options, int64_t requests_size,
      int64_t request_index) const;

  // Returns the stream executors assigned to the replicas represented by the
  // given device handle. Each device_handle is a virtual replicated device that
  // represents a set of physical devices for the replicas.
  StatusOr<std::vector<se::StreamExecutor*>> Replicas(
      const Backend& backend, const DeviceHandle& device_handle) const;

  // Builds an Executable for the given parameters.
  StatusOr<std::unique_ptr<Executable>> BuildExecutable(
      const HloModuleProto& module_proto,
      std::unique_ptr<HloModuleConfig> module_config, Backend* backend,
      se::StreamExecutor* executor, const Compiler::CompileOptions& options,
      bool run_backend_only = false);
```