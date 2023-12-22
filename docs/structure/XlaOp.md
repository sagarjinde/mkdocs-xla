# **XlaOp**

This represents an instruction that has been enqueued using the [XlaBuilder](XlaBuilder.md).
This is used to pass to subsequent computations that depends upon the
instruction as an operand.

### Attributes Overview

```cpp
  // < 0 means "invalid handle".
  int64_t handle_;

  // Not owned. Non-null for any handle returned by XlaBuilder, even if the
  // handle is invalid.
  XlaBuilder* builder_;
```

!!! note
    handle_ is used to point to its corresponding HloInstructionProto via handle_to_index_ in XlaBuilder.