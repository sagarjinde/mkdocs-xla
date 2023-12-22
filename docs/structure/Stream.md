# **Stream**

!!! warning
    - Still have doubts. Requires further study.

!!! question
    - Does each Stream end up being a set of Kernels down the line?
    - Is Stream just a predefined set of dependent Kernels?

Represents a stream of dependent computations on a GPU device.

The operations within a stream execute linearly and asynchronously until
BlockHostUntilDone() is invoked, which synchronously joins host code with
the execution of the stream.

### Attributes Overview

- Stores [StreamExecutor](StreamExecutor.md) which executes Stream on device

```cpp
  // The StreamExecutor that supports the operation of this stream.
  StreamExecutor *parent_;
```

### Functions Overview

- Define Streams
- Entrains [Kernel](Kernel.md)s into Stream

```cpp
  // Initialize the stream. This must be performed before entraining any other
  // operations.
  Stream &Init() TF_LOCKS_EXCLUDED(mu_);

  // Entrains onto the stream of operations: a kernel launch with the given
  // (variadic) parameters for the invocation.
  template <typename... Params, typename... Args>
  tsl::Status ThenLaunch(ThreadDim thread_dims, BlockDim block_dims,
                         const TypedKernel<Params...> &kernel, Args... args);

  Stream &ThenConvolve(const dnn::BatchDescriptor &input_descriptor,
                       const DeviceMemory<float> &input_data,
                       const dnn::FilterDescriptor &filter_descriptor,
                       const DeviceMemory<float> &filter_data,
                       const dnn::ConvolutionDescriptor &convolution_descriptor,
                       const dnn::BatchDescriptor &output_descriptor,
                       DeviceMemory<float> *output);

  Stream &ThenMatMul(const DeviceMemory<float> &input_data,
                     const DeviceMemory<float> &weights,
                     const dnn::BatchDescriptor &input_dimensions,
                     const dnn::BatchDescriptor &output_dimensions,
                     DeviceMemory<float> *output_data);
```