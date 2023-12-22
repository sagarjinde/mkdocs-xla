# **XlaBuilder**

A convenient interface for building up computations.

!!! note
    This is the class which build XLA graph ([XlaComputation](XlaComputation.md))

### Attributes Overview

- Stores instructions used to build [XlaComputation](XlaComputation.md)

```cpp
  // The instructions of this computation.
  std::deque<HloInstructionProto> instructions_;

  // A map from XlaOp::Handle to the index in the instructions_ vector where the
  // instruction is held.
  absl::flat_hash_map<int64_t, int64_t> handle_to_index_;

  // The unique parameter numbers.
  absl::flat_hash_set<int64_t> parameter_numbers_;
```

### Functions Overview

- Constructs [XlaOp](XlaOp.md)s
- Build [XlaComputation](XlaComputation.md)
- Get computation details like [ProgramShape](ProgramShape.md)

```cpp
  XlaOp Reshape(XlaOp operand, absl::Span<const int64_t> new_sizes,
                int64_t inferred_dimension = -1);

  XlaOp Slice(XlaOp operand, absl::Span<const int64_t> start_indices,
              absl::Span<const int64_t> limit_indices,
              absl::Span<const int64_t> strides);

  XlaOp Conv(
      XlaOp lhs, XlaOp rhs, absl::Span<const int64_t> window_strides,
      Padding padding, int64_t feature_group_count = 1,
      int64_t batch_group_count = 1,
      const PrecisionConfig* precision_config = nullptr,
      std::optional<PrimitiveType> preferred_element_type = std::nullopt);

  // Builds the computation with the requested operations, or returns a non-ok
  // status. Note that all ops that have been enqueued will be moved to the
  // computation being returned. The root of the computation will be the last
  // added operation.
  StatusOr<XlaComputation> Build(bool remove_dynamic_dimensions = false);

  // Returns the shape of the given op.
  StatusOr<Shape> GetShape(XlaOp op) const;

  // Returns the (inferred) result for the current computation's shape. This
  // assumes the root instruction is the last added instruction.
  StatusOr<ProgramShape> GetProgramShape() const;
```