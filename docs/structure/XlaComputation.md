# **XlaComputation**

The computation graph that the user builds up with the [XlaBuilder](XlaBuilder.md).

### Attributes Overview

```cpp
  int64_t unique_id_;
  HloModuleProto proto_;
```

!!! note
    - HloModuleProto stores all information that [HloModule](HloModule.md) contains.
    - So, information like HloComputation, entry computation is stored