# **Executable**

A given platform's compiler will produce an Executable -- this is a uniform
interface that is used for launching compiled programs across platforms

!!! note
    - This is where actual execution of Ops takes place

### Attributes Overview

```cpp
  // HloModule this was compiled from. BufferAssignment keeps pointers to
  // HloInstructions owned by the HloModule so we need to keep the HloModule
  // around if we keep the BufferAssignment around.
  //
  // This member may be nullptr, if the given executable type doesn't need it
  // for execution.
  const std::shared_ptr<HloModule> hlo_module_;

  // The serialized HLO proto. Non-null only if dumping snapshots is enabled.
  std::unique_ptr<HloProto const> hlo_proto_;
```

### Functions Overview

```cpp
  // Enqueues the compilation result on the provided stream, passing the given
  // arguments. This call is blocking and returns after the execution is done.
  // Returns a shaped buffer containing the result of the computation.
  StatusOr<ScopedShapedBuffer> ExecuteOnStream(
      const ServiceExecutableRunOptions* run_options,
      absl::Span<const ShapedBuffer* const> arguments,
      HloExecutionProfile* hlo_execution_profile);
```

!!! note
    - ServiceExecutableRunOptions contains Stream and options for running Executable.