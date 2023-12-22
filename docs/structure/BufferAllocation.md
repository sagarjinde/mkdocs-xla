# **BufferAllocation**

This class abstracts an allocation of contiguous memory which can hold the
values described by LogicalBuffers. Each LogicalBuffer occupies a sub-range
of the allocation, represented by a Slice. A single BufferAllocation may hold
LogicalBuffers with disjoint liveness, which may have overlapping Slices. A
single BufferAllocation may also hold LogicalBuffers with overlapping
liveness, which must have disjoint Slices.

**Example:** 
BufferAllocation (contiguous memory):

[ -- [LogicalBuffer1 (Slice1)] -------- [LogicalBuffer2 (Slice2)] ---------------- [LogicalBuffer3 (Slice3)] ---- ]