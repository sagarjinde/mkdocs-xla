# **Shape**

A shape describes the number of dimensions in a array, the bounds of each
dimension, and the primitive component type.

### Attributes Overview

- Stores details about element type, dimensions and [Layout](Layout.md)

```cpp
  // The element type of this shape (tuple, array, etc).
  PrimitiveType element_type_ = PRIMITIVE_TYPE_INVALID;

  // The array bounds of the dimensions. This is nonempty only for array
  // shapes. For a dynamically-sized dimension, the respective value in this
  // vector is an inclusive upper limit of the array bound.
  DimensionVector dimensions_;

  // This vector is the same size as 'dimensions_' and indicates whether the
  // respective dimension is dynamically sized.
  absl::InlinedVector<bool, InlineRank()> dynamic_dimensions_;

  // The tuple element subshapes. This is nonempty only for tuple shapes.
  std::vector<Shape> tuple_shapes_;

  // The layout of the shape. Only relevant for arrays.
  std::optional<Layout> layout_;
```

### Functions Overview

```cpp
  // Prints a human-readable string that represents the given shape, with or
  // without layout. e.g. "F32[42,12] {0, 1}" or "F32[64]".
  void Print(Printer* printer, bool print_layout = false) const;
```