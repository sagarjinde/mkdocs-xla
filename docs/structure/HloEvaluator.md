# **HloEvaluator**

Responsible for evaluating HLO and obtain literal as the evaluation results.

!!! note
    - HloEvaluator is used by interpreter to execute HLO operators.
    - Backends (CPU/GPU) use [Executable](Executable.md) to execute HLO operators.

!!! note
    - Here, evaluating HLO means to actually perform the HLO operation.

### Attributes Overview

- Store input and evaluated [Literal](Literal.md)s

```cpp
  // Tracks the HLO instruction and its evaluated literal result.
  bsl::node_hash_map<const HloInstruction*, Literal> evaluated_;

  // Caches pointers to input literals
  std::vector<const Literal*> arg_literals_;
```

### Functions Overview

- Evaluate HLO Operations and return result [Literal](Literal.md)

```cpp
  // Evaluates an HLO module and an array of pointers to literals.  Returns the
  // evaluated result as a literal if successful.
  StatusOr<Literal> Evaluate(const HloModule& module,
                             absl::Span<const Literal* const> arg_literals) {
    return Evaluate(*module.entry_computation(), arg_literals);
  }

  StatusOr<Literal> Evaluate(const HloComputation& computation,
                             absl::Span<const Literal* const> arg_literals);

  StatusOr<Literal> Evaluate(
      const HloInstruction* instruction,
      bool recursively_evaluate_nonconstant_operands = false);

  Status HandleConcatenate(const HloInstruction* concatenate) override;
  Status HandleReshape(const HloInstruction* reshape) override;
  Status HandleTranspose(const HloInstruction* transpose) override;
```

!!! note
    - The result of evaluation is stored in ```evaluated_```