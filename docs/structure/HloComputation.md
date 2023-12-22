# **HloComputation**

You can think of an HloComputation like a function.  It has some inputs
(parameters) and returns exactly one value (the value of its root node).  If
you want to return multiple values, you can return a tuple.
The instructions inside of a computation do not have an explicit total order.
Instead, they have a partial order determined by their data and control
dependencies.

### Attributes Overview

- Store and access [HloInstruction](HloInstruction.md)s in HloComputation.

```cpp
  HloInstruction* root_instruction_;

  // If this computation is a fusion computation, this field points to the
  // corresponding fusion instruction (if it is live). Otherwise, this is null.
  HloInstruction* fusion_instruction_;

  // Determines whether this computation is a fusion computation. A fusion
  // computation ordinarily also has a non-null fusion_instruction_. However, if
  // a fusion instruction is removed during compilation, the fusion computation
  // becomes unreachable, and its fusion_instruction_ is set to null. We still
  // need to regard such computations as fusion computations for HLO scheduling
  // purposes.
  bool is_fusion_computation_;

  // Module containing this computation.
  HloModule* parent_ = nullptr;

  // Store instructions in std::list as they can be added and removed
  // arbitrarily and we want a stable iteration order. Keep a map from
  // instruction pointer to location in the list for fast lookup.
  InstructionList instructions_;
  absl::flat_hash_map<const HloInstruction*, InstructionList::iterator>
      instruction_iterators_;

  HloInstruction::InstructionVector param_instructions_;
```

!!! note

    - Root instruction is the last instruction of the computation.
    - Parameter instructions are the inputs to the computation.


### Functions Overview

- Build HloComputation from [HloInstruction](HloInstruction.md)s.
- Perform operations on [HloInstruction](HloInstruction.md).

```cpp
  // Build and return an HloComputation. The parameter root_instruction
  // specifies the already-added instruction to use as the root. If
  // root_instruction is nullptr then use the last added instruction as the
  // root.
  std::unique_ptr<HloComputation> Build(
      HloInstruction* root_instruction = nullptr);

  // Add an instruction to the computation. The computation takes ownership of
  // the instruction.
  HloInstruction* AddInstruction(std::unique_ptr<HloInstruction> instruction,
                                 absl::string_view new_name = "");

  // Remove an instruction from the computation. The instruction must have no
  // users. Instruction is deallocated with this call.
  Status RemoveInstruction(HloInstruction* instruction);

  // Replaces old instruction with newly created instruction. Removes old
  // instruction from computation. Updates uses and root instruction.
  Status ReplaceWithNewInstruction(
      HloInstruction* old_instruction,
      std::unique_ptr<HloInstruction> new_instruction);

  // Computes and returns the ProgramShape of this computation (shape of
  // parameters and result with layout).
  ProgramShape ComputeProgramShape(bool include_ids = true) const;
```