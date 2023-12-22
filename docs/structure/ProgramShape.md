# **ProgramShape**

Shape of the parameters and output of an XLA computation.

### Attributes Overview

```cpp
  // The shapes of the parameters of the computation represented by this object.
  std::vector<Shape> parameters_;

  // The names of the parameters of the computation represented by this object.
  std::vector<std::string> parameter_names_;

  // The shape of the result of the computation represented by this object.
  Shape result_;
```