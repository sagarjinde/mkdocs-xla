---
hide:
  - toc
---

# **Call Graph**

``` mermaid
sequenceDiagram
    
    actor user

    user ->> xla_builder: XlaBuilder::Build
    xla_builder -->> user: XlaComputation
    user ->> client: ExecuteAndTransfer (XlaComputation)
    client ->> service: Execute (XlaComputation, ExecutionOptions)
    service ->> backend: GetExecutors (ExecutionOptions)
    backend -->> service: StreamExecutor
    service ->> cpu_compiler: Compile (HloModule, StreamExecutor)
    cpu_compiler ->> llvm_compiler: Compile (HloModule, StreamExecutor)
    llvm_compiler ->> cpu_compiler: RunHloPasses (HloModule)
    cpu_compiler -->> llvm_compiler: HloModule
    llvm_compiler ->>+ cpu_compiler: RunBackend (HloModule)
    cpu_compiler ->> hlo_memory_scheduler: ScheduleModule (HloModule)
    hlo_memory_scheduler -->> cpu_compiler: HloSchedule
    cpu_compiler ->> buffer_assignment: BufferAssigner::Run (HloModule, HloSchedule)
    buffer_assignment -->> cpu_compiler: BufferAssignment
    cpu_compiler ->> ir_emitter: IrEmitter::EmitComputation (HloModule, HloSchedule)
    ir_emitter -->> cpu_compiler: llvm::Function
    cpu_compiler ->> cpu_executable: CpuExecutable::Create (jit, BufferAssignment, HloModule, llvm::Function)
    cpu_executable -->> cpu_compiler: CpuExecutable
    cpu_compiler -->>- llvm_compiler: Executable
    llvm_compiler -->> cpu_compiler: Executable
    cpu_compiler -->> service: Executable
    service ->> executable: ExecuteOnStreams (Executable)
    executable -->> service: GlobalDataHandle
    service -->> client: GlobalDataHandle
    client -->>  user: GlobalData

```

### **How to read the Call Graph?**

``` mermaid
sequenceDiagram
    
filename_1 ->> filename_2: functionCalled (parameter1, parameter2, ..)
filename_2 -->> filename_1: returnedVariable

```