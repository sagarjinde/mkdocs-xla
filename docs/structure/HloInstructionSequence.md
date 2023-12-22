# **HloInstructionSequence**

Class representing a sequence of [HloInstruction](HloInstruction.md)s for a [HloComputation](HloComputation.md).

### Attributes Overview

- Stores [HloInstruction](HloInstruction.md) sequence for a [HloComputation](HloComputation.md)

```cpp
  // The sequence as HloInstructions.
  std::vector<HloInstruction*> instruction_sequence_;

  // The sequence of HLO instructions, represented by their unique IDs. The
  // sequence is stored as both HloInstructions and unique IDs because the
  // sequence may be referenced after transformations to the HLO graph and HLO
  // pointers can be invalidated or recycled in this process (see
  // HloSchedule::Update).
  std::vector<int> id_sequence_;
```

### Functions Overview

- Modify sequence

```cpp
  // Adds the instruction to the end of the sequence.
  void push_back(HloInstruction* instruction);

  // Removes the instruction from the sequence.
  void remove_instruction(HloInstruction* instruction);

  // Replaces the old instruction with the new instruction in the sequence.
  void replace_instruction(HloInstruction* old_instruction,
                           HloInstruction* new_instruction);
```

