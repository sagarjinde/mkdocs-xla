---
hide:
  - toc
---

# **XLA Structure**

``` mermaid
graph TD;
    XLA --> HloModule;
    HloModule --> HloComputation;
    HloComputation --> HloInstruction;
    HloInstruction --> Shape;
    Shape --> Layout;

    XLA --> HloSchedule;
    HloSchedule --> HloInstructionSequence;

    XLA --> XlaBuilder;
    XlaBuilder --> ProgramShape;
    XlaBuilder --> XlaComputation;
    XlaBuilder --> XlaOp;

    XLA --> HloEvaluator;
    HloEvaluator --> Literal;

    XLA --> Client;
    Client --> Service;
    Service --> Executable;
    Executable --> Stream;
    Stream --> StreamExecutor;
    StreamExecutor --> Kernel;

    XLA --> BufferAssignment;
    BufferAssignment --> BufferAllocation;



    click HloModule "../structure/HloModule"
    click HloComputation "../structure/HloComputation"
    click HloInstruction "../structure/HloInstruction"
    click Shape "../structure/Shape"
    click Layout "../structure/Layout"

    click HloSchedule "../structure/HloSchedule"
    click HloInstructionSequence "../structure/HloInstructionSequence"

    click XlaBuilder "../structure/XlaBuilder"
    click ProgramShape "../structure/ProgramShape"
    click XlaComputation "../structure/XlaComputation"
    click XlaOp "../structure/XlaOp"

    click HloEvaluator "../structure/HloEvaluator"
    click Literal "../structure/Literal"

    click Client "../structure/Client"
    click Service "../structure/Service"
    click Executable "../structure/Executable"
    click Stream "../structure/Stream"
    click StreamExecutor "../structure/StreamExecutor"
    click Kernel "../structure/Kernel"

    click BufferAssignment "../structure/BufferAssignment"
    click BufferAllocation "../structure/BufferAllocation"
```

### **How to read the XLA Structure?**

``` mermaid
graph TD;
    Class1 --> |Uses|Class2
```

Example: ```HloInstruction``` Uses ```Shape``` to represent the shape of its result.

!!! note
    Not all the "Uses" relation is captured here. Relations like "```HloInstructionSequence``` Uses ```HloInstruction``` to represent instructions present in sequence" is not captured as this is "expected". This was done to keep the graph clean.