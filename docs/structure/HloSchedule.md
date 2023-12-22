# **HloSchedule**

A class representing a sequential schedule of instructions for an [HloModule](HloModule.md).
A complete HLO schedule contains an instruction sequence for every
non-fusion computation in the [HloModule](HloModule.md).

### Attributes Overview

- Stores sequence of [HloInstruction](HloInstruction.md)s for multiple [HloComputation](HloComputation.md) using [HloInstructionSequence](HloInstructionSequence.md)

```cpp
  const HloModule* module_;

  // A map from computation unique ID to instruction sequence. Unique IDs are
  // used rather than HloComputation pointers because HLO pointers are not
  // unique across HLO transformations because pointers may be recycled.
  absl::flat_hash_map<int64_t, HloInstructionSequence> sequences_;
```

### Functions Overview

- Modify the sequence of [HloInstruction](HloInstruction.md)s

```cpp
  // Sets the sequence for the given computation to the given sequence.
  void set_sequence(const HloComputation* computation,
                    absl::Span<HloInstruction* const> sequence);

  // Removes the instruction from the computation's sequence.
  void remove_instruction(const HloComputation* computation,
                          HloInstruction* instruction) {
    sequences_[computation->unique_id()].remove_instruction(instruction);
  }

  // Replaces the old instruction with the new instruction in the computation's
  // sequence.
  void replace_instruction(const HloComputation* computation,
                           HloInstruction* old_instruction,
                           HloInstruction* new_instruction) {
    sequences_[computation->unique_id()].replace_instruction(old_instruction,
                                                             new_instruction);
  }
```