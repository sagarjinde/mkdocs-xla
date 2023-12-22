# **BufferAssignment**

This class encapsulates an assignment of the LogicalBuffers in an XLA
module to a set of [BufferAllocation](BufferAllocation.md)s.

### Attributes Overview

- Stores relation between LogicalBuffers and [BufferAllocation](BufferAllocation.md)s

```cpp
  // The vector of buffer allocations. Indexed by BufferAllocation::Index.
  std::vector<BufferAllocation> allocations_;

  // Maps Buffers to the index of the BufferAllocation which holds the buffer.
  absl::flat_hash_map<const HloValue*, BufferAllocation::Index>
      allocation_index_for_value_;

  const HloModule* module_;

  const std::unique_ptr<HloOrdering> hlo_ordering_;
```

### Functions Overview

- Perform operations over [BufferAllocation](BufferAllocation.md)

```cpp
  // Returns whether the given buffer has been assigned an allocation.
  bool HasAllocation(const HloValue& value) const;

  // Returns the allocation that a particular LogicalBuffer has been assigned
  // to. CHECKs if buffer has not been assigned an allocation.
  const BufferAllocation& GetAssignedAllocation(const HloValue& value) const;

  // Adds a LogicalBuffer to the set assigned to the given allocation.
  void AddAssignment(BufferAllocation* allocation, const HloBuffer& buffer,
                     int64_t offset, int64_t size);
```