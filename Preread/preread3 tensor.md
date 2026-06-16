- PyTorch is an open-source machine learning and deep learning library used to build and train artificial neural networks.
- tensors are data structures (similar to arrays)
- tensor initialization:
  1. directly from data 
   x_data = torch.tensor(data)
  2. from numpy arrays
   x_np = torch.from_numpy(np_array)
  3. from other tensors- it can either retain the data of the first tensor or overwrite it
   x_ones = torch.ones_like(x_data) - this creates a new tensor with the same shape as x_data tensor but its filled with ones 
   rand_like() - this fills the tensor with random numbers
- a tupple is used to define the size of a tensor
- Tensor attributes describe their shape, datatype, and the device on which they are stored.
- Tensor API is very simalr to NumPy API
- different tensor operations:
    1. tensor[:,1] = 0  -  Set second column to zero
    2. torch.cat(..., dim=1) - join tensors side by side 
                                          dim=1 → join column-wise
    3. tensor * tensor  - element wise multiplication 
    4. tensor @ tensor.T  -  matrix multiplication
    5. tensor.add_(5)  -  add five
- pytorch and numpy can work together 
- t = torch.ones(5)
  n = t.numpy()
t is a PyTorch tensor, n is a NumPy array. Both point to the same data in memory.
- if you change any one of them, the other one also changes 
- This sharing works only when: the tensor is on the CPU, not on the GPU
- If the tensor is on a GPU, you must first move it to CPU:
- n = tensor.cpu().numpy()