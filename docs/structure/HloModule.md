# **HloModule**

HloModule is the top-level unit in the HLO IR.  It corresponds to a whole
"program".  Running a module, from beginning to end, is the only way to run
an XLA program.
A module contains one "entry computation"; this HloComputation is like main()
in a C program.  The result of running the module is the result of running
this computation.

### Attributes Overview

- Store and access [HloComputation](HloComputation.md)s in HloModule.
- [HloSchedule](HloSchedule.md) to store sequential order of instructions.

```cpp
  CopyOnWrite<HloModuleConfig> config_;
  HloComputation* entry_computation_ = nullptr;
  std::vector<std::unique_ptr<HloComputation>> computations_;

  // The HloSchedule of the module. The schedule if it exists contains a
  // sequential order of instructions for each non-fusion computation in the
  // module.
  std::optional<HloSchedule> schedule_;
```

### Functions Overview

- Perform operations on [HloComputation](HloComputation.md)s present in HloModule.

```cpp
  // Adds an entry computation to the module. A module can only have one entry
  // computation. Returns a pointer to the newly added computation.
  HloComputation* AddEntryComputation(
      std::unique_ptr<HloComputation> computation);

  // Performs a deep clone of the computation, by recursively cloning all
  // the called computations as well. If the clone context is specified, it
  // will be populated with the cloned object mappings.
  HloComputation* DeepCloneComputation(HloComputation* computation,
                                       HloCloneContext* context = nullptr);

  // Replaces all uses of computations that are keys of 'replacements' with
  // the corresponding values in 'replacements'. Replaces the entry computation,
  // if applicable.
  void ReplaceComputations(
      const absl::flat_hash_map<HloComputation*, HloComputation*>&
          replacements);

  // Returns the root instruction shape of entry computation.
  // Precondition: entry_computation_ is not nullptr.
  const Shape& result_shape() const {
    CHECK_NE(nullptr, entry_computation_);
    return entry_computation()->root_instruction()->shape();
  }
```