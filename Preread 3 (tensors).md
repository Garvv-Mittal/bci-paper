#MAIN PURPOSE OF PYTORCH
- A replacement for NumPy to use the power of GPUs and other accelerators.
- An automatic differentiation library that is useful to implement neural networks.
---------------------------------------------------------------------
---------------------------------------------------------------------
---------------------------------------------------------------------
TENSORS
Tensors are a specialized data structure that are very similar to arrays and matrices. In PyTorch, we use tensors to encode the inputs and outputs of a model, as well as the model’s parameters.
     similar to NumPy's NDARRAYS, except tensors can run on GPUs or other specialized hardware to accelerate computing.

Why we use them: In PyTorch, everything is a tensor. We use them to encode:
- The inputs (e.g., image pixels, text tokens).
- The outputs (e.g., predicted labels).
- The model parameters (weights and biases that the AI learns during training).

///////////////

1. Creating Tensors (Tensor Initialization)
There are four primary ways to create a tensor depending on where your data is coming from.
......
A. Directly from Raw Data
You can pass a Python list or tuple directly into torch.tensor().

data = [[1, 2], [3, 4]]
x_data = torch.tensor(data)
.........
B. From a NumPy Array
If you already have data in NumPy, you can turn it into a PyTorch tensor seamlessly.

np_array = np.array([[1, 2], [3, 4]])
x_np = torch.from_numpy(np_array)
..........
C. From Another Tensor
You can create a new tensor by copying the shape and datatype of an existing tensor. This is useful when you want to create a mask or reset values while keeping the structure intact.

# Overrides the datatype to float while keeping the same shape, filled with random numbers
x_rand = torch.rand_like(x_data, dtype=torch.float) 
...........
D. With Random or Constant Values
You can define a specific shape (tuple) and fill it with random numbers, ones, or zeros.

Python
shape = (2, 3) # 2 rows, 3 columns

rand_tensor = torch.rand(shape)    # Uniform random numbers between 0 and 1
ones_tensor = torch.ones(shape)    # Filled completely with 1.0
zeros_tensor = torch.zeros(shape)  # Filled completely with 0.0


//////////////////////

2. The 3 Essential Tensor Attributes
Every single tensor has three main properties that tell you what it is, what's inside it, and where it lives.

Python
tensor = torch.rand(3, 4)

print(f"Shape of tensor: {tensor.shape}")   # e.g., torch.Size([3, 4]) -> Dimensions
print(f"Datatype of tensor: {tensor.dtype}") # e.g., torch.float32 -> Type of numbers
print(f"Device tensor is on: {tensor.device}") # e.g., cpu or cuda:0 -> Hardware location
Note: By default, all tensors are created on the CPU. You must explicitly tell PyTorch to move them if you want to use a GPU.


////////////////////

3. Tensor Operations
PyTorch supports over 100 operations (math, linear algebra, slicing, etc.). They run exactly like NumPy, but drastically faster if moved to a GPU.
............
A. Moving Tensors to the GPU
Before doing heavy math, you should check if a GPU is available and move your tensor there using .to().

Python
# Check if CUDA (NVIDIA GPU driver) is available
if torch.cuda.is_available():
    tensor = tensor.to('cuda')
    print("Tensor moved to GPU successfully!")
...........
B. Indexing and Slicing
Standard Python/NumPy slicing rules apply.

Python
tensor = torch.ones(4, 4)

print('First row: ', tensor[0])        # Gets row index 0
print('First column: ', tensor[:, 0])   # Gets column index 0
print('Last column:', tensor[..., -1]) # The "..." means select all preceding dimensions



/////////////////

C. Joining Tensors (Concatenation)
Use torch.cat to glue a list of tensors together along a specific dimension (dim).

Python
# Sticking tensors together side-by-side (column-wise)
t1 = torch.cat([tensor, tensor, tensor], dim=1) 

/////////////////

D. Arithmetic Operations (Multiplication)
There is a massive mathematical difference between Element-wise multiplication and Matrix multiplication.

Element-wise product: Multiplies numbers that are in the exact same positions.

Python
# These three methods do the exact same thing:
z1 = tensor * tensor
z2 = tensor.mul(tensor)


z3 = torch.empty_like(tensor)

torch.mul(tensor, tensor, out=z3)
Matrix Multiplication (Dot Product): Standard linear algebra row-by-column multiplication.

Python
    # These two methods do the exact same thing (tensor.T is the Transpose):
    y1 = tensor @ tensor.T
    y2 = tensor.matmul(tensor.T)


////////////


E. In-Place Operations (_ suffix)
Operations that have a trailing underscore _ modify the tensor directly in place, meaning they overwrite the original data instead of creating a new copy.

Python
x = torch.ones(2, 2)
x.add_(5) 
print(x) # x is now filled with 6.0 instead of 1.0




///////////////////////

4. The NumPy Bridge (Memory Sharing)
Tensors on the CPU and NumPy arrays can be converted back and forth effortlessly because they share the underlying memory locations. Modifying one will automatically modify the other.

A. Tensor to NumPy Array
Python
t = torch.ones(5)
n = t.numpy() # Convert to NumPy

# Changing the tensor changes the NumPy array automatically!
t.add_(1) 
print(f"t: {t}") # [2, 2, 2, 2, 2]
print(f"n: {n}") # [2, 2, 2, 2, 2]
B. NumPy Array to Tensor
Python
n = np.ones(5)
t = torch.from_numpy(n) # Convert to Tensor

# Changing the NumPy array changes the tensor automatically!
np.add(n, 1, out=n)
print(f"n: {n}") # [2, 2, 2, 2, 2]
print(f"t: {t}") # [2, 2, 2, 2, 2]