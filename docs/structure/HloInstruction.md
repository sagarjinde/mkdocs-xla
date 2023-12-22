# **HloInstruction**

HLO instructions are the atomic unit of the high-level compiler's IR.

### Attributes Overview

- Stores HloInstruction type of operation, operators, users
- Which [HloComputation](HloComputation.md) it belongs to
- [Shape](Shape.md) of the result

```cpp
  int unique_id_;  // Unique to this HloInstruction within a HloModule

  // Opcode for this instruction.
  HloOpcode opcode_;

  // Instruction operands.
  InstructionVector operands_;

  // The set of control predecessors of this instruction.
  std::vector<HloInstruction*> control_predecessors_;

  // The users of this instruction. Users are HLOs where this instruction is an
  // operand. The vector users_ and the map user_map_ contain identical members.
  // The map enables fast membership testing and the vector enables fast, stable
  // iteration. The value in the map contains the index of the instruction in
  // the vector what enables fast removal.
  std::vector<HloInstruction*> users_;
  absl::flat_hash_map<const HloInstruction*, int64_t> user_map_;

  // The set of control successors of this instruction.
  std::vector<HloInstruction*> control_successors_;

  // The computation in which this instruction is contained.
  HloComputation* parent_ = nullptr;

  // Result shape of this instruction.
  Shape shape_;
```

### Functions Overview

- Create HloInstruction
- Modify attributes of HloInstruction

```cpp
  // Creates a pad instruction, where the operand is padded on the edges and
  // between the elements with the given padding value.
  static std::unique_ptr<HloInstruction> CreatePad(
      const Shape& shape, HloInstruction* operand,
      HloInstruction* padding_value, const PaddingConfig& padding_config);

  // Creates a reshape instruction, where the operand is flattened row-major
  // order and then reshaped to the given result shape.
  static std::unique_ptr<HloInstruction> CreateReshape(
      const Shape& shape, HloInstruction* operand,
      int64_t inferred_dimension = -1);

  // Creates a transpose instruction which permutes the operand dimensions.
  static std::unique_ptr<HloInstruction> CreateTranspose(
      const Shape& shape, HloInstruction* operand,
      absl::Span<const int64_t> dimensions);

  // Replaces the use of this instruction in "user" with "new_producer". Note
  // that there might be multiple uses of this instruction in "user"; all will
  // be replaced.
  Status ReplaceUseWith(HloInstruction* user, HloInstruction* new_producer);

  // Replaces the specified operand with new_operand. The old and new operands
  // must have compatible shapes ignoring floating-point precision.
  Status ReplaceOperandWith(int64_t operand_num, HloInstruction* new_operand);

  // Clones the HLO instruction. The clone will have the same opcode, shape, and
  // operands.
  std::unique_ptr<HloInstruction> Clone(
      const std::string& suffix = "clone",
      HloCloneContext* context = nullptr) const;
```