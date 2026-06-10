
## Table of Contents
1. [Part 1 — Tensors](#part-1-->tensors)
2. [Part 2 — torch.autograd](#part-2-->a gentle introduction to torch.autograd)
3. [Part 3 — Neural Networks](#part-3-->neural-networks)
4. [Part 4 — Training a Classifier](#part-4-->training a classifier)
5. [Quick Reference Cheatsheet](#quick-reference-cheatsheet)



## Part 1 — Tensors

### What is a Tensor?
A **tensor** is a multi-dimensional array similar to NumPy's `ndarray`, but with two superpowers:
- Can run on **GPU** (accelerated computing)
- Supports **automatic differentiation** (used in training neural networks)

```python
import torch
import numpy as np
```


### 1.1 Tensor Initialization

**From data directly:**
```python
data = [[1, 2], [3, 4]]
x = torch.tensor(data)
```

**From a NumPy array:**
```python
np_array = np.array(data)
x_np = torch.from_numpy(np_array)
# NOTE: tensor and np_array share memory — changing one changes the other
```

**From another tensor (inherits shape and dtype):**
```python
x_ones = torch.ones_like(x)      # same shape, all 1s
x_rand = torch.rand_like(x, dtype=torch.float)  # same shape, random floats
```

**With fixed shape:**
```python
shape = (2, 3)
rand_tensor  = torch.rand(shape)
ones_tensor  = torch.ones(shape)
zeros_tensor = torch.zeros(shape)
```

---

### 1.2 Tensor Attributes

```python
tensor = torch.rand(3, 4)

print(tensor.shape)   # torch.Size([3, 4])
print(tensor.dtype)   # torch.float32
print(tensor.device)  # cpu  (or cuda:0 if on GPU)
```

---

### 1.3 Moving to GPU

```python
device = torch.accelerator.current_accelerator().type if torch.accelerator.is_available() else 'cpu'
tensor = tensor.to(device)
```

---

### 1.4 Tensor Operations

**Indexing and slicing (same as NumPy):**
```python
tensor = torch.ones(4, 4)
tensor[:, 1] = 0   # set entire column 1 to 0
print(tensor)
```

**Joining tensors:**
```python
t1 = torch.cat([tensor, tensor, tensor], dim=1)  # concatenate along columns
# torch.stack() is different — creates a new dimension
```

**Element-wise multiplication:**
```python
result = tensor * tensor           # operator style
result = tensor.mul(tensor)        # method style
```

**Matrix multiplication:**
```python
result = tensor @ tensor.T         # operator style
result = tensor.matmul(tensor.T)   # method style
```

**In-place operations (suffix `_`):**
```python
tensor.add_(5)   # modifies tensor in place
# ⚠️ Avoid in-place ops during gradient computation — they destroy history
```

**Single-element tensor to Python scalar:**
```python
agg = tensor.sum()
val = agg.item()   # converts to Python float/int
```

---

### 1.5 NumPy Bridge

CPU tensors and NumPy arrays **share the same memory**.

```python
# Tensor → NumPy
t = torch.ones(5)
n = t.numpy()
t.add_(1)          # modifying t also modifies n !

# NumPy → Tensor
n = np.ones(5)
t = torch.from_numpy(n)
np.add(n, 1, out=n)  # modifying n also modifies t !
```

> **Key insight:** This is only true for CPU tensors. GPU tensors don't share memory with NumPy.

---

## Part 2 — A Gentle Introduction to `torch.autograd`

### What is Autograd?
`torch.autograd` is PyTorch's **automatic differentiation engine**. It automatically computes gradients, which are needed for backpropagation during neural network training.

---

### 2.1 Training — Two Steps

| Step | What happens |
|------|-------------|
| **Forward Pass** | Network makes a prediction by passing input through all layers |
| **Backward Pass (Backprop)** | Error is propagated back; gradients computed; weights updated |

---

### 2.2 Basic Usage

```python
import torch
from torchvision.models import resnet18, ResNet18_Weights

model = resnet18(weights=ResNet18_Weights.DEFAULT)
data   = torch.rand(1, 3, 64, 64)   # fake image: 1 sample, 3 channels, 64x64
labels = torch.rand(1, 1000)         # fake labels

# Forward pass
prediction = model(data)

# Compute loss
loss = (prediction - labels).sum()

# Backward pass — computes gradients
loss.backward()

# Optimizer step — updates weights using gradients
optim = torch.optim.SGD(model.parameters(), lr=1e-2, momentum=0.9)
optim.step()
```

---

### 2.3 How Gradients are Tracked

```python
a = torch.tensor([2., 3.], requires_grad=True)
b = torch.tensor([6., 4.], requires_grad=True)

# Define a function
Q = 3*a**3 - b**2

# Backward pass — pass gradient of Q w.r.t itself
external_grad = torch.tensor([1., 1.])
Q.backward(gradient=external_grad)

# Check gradients: dQ/da = 9a², dQ/db = -2b
print(a.grad)   # tensor([36., 27.])
print(b.grad)   # tensor([-12.,  -8.])
```

> `requires_grad=True` tells PyTorch: *"track every operation on this tensor"*

---

### 2.4 Computational Graph (DAG)

PyTorch builds a **Directed Acyclic Graph (DAG)** of operations during the forward pass:
- **Leaves** = input tensors
- **Roots** = output tensors
- When `.backward()` is called, PyTorch traces back from root to leaves using the **chain rule**

**Important:** The DAG is recreated from scratch on every forward pass. This is why PyTorch supports dynamic models (if/else, loops, etc.)

---

### 2.5 Freezing Parameters (`requires_grad=False`)

Useful for **transfer learning** — freeze pretrained layers, only train the new head.

```python
model = resnet18(weights=ResNet18_Weights.DEFAULT)

# Freeze all layers
for param in model.parameters():
    param.requires_grad = False

# Replace final layer (unfrozen by default)
model.fc = nn.Linear(512, 10)   # 10 new classes

# Only model.fc parameters will be updated
optimizer = torch.optim.SGD(model.parameters(), lr=1e-2, momentum=0.9)
```

> **Why this matters for BCI:** When adapting a pretrained EEG model to a new subject, you freeze lower layers and only finetune the top classifier — this is exactly the domain adaptation idea in your research.

---

### 2.6 Disabling Gradient Tracking

```python
# Context manager — no gradients computed inside
with torch.no_grad():
    output = model(data)

# Or set permanently
tensor.requires_grad_(False)
```

---

## Part 3 — Neural Networks

### What is `torch.nn`?
`torch.nn` provides all the building blocks for neural networks:
- **Layers:** `nn.Linear`, `nn.Conv2d`, `nn.ReLU`, etc.
- **Loss functions:** `nn.CrossEntropyLoss`, `nn.MSELoss`, etc.
- **Container:** `nn.Module` — base class for all models

---

### 3.1 Defining a Neural Network

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        # Convolutional layers
        self.conv1 = nn.Conv2d(1, 6, 5)    # 1 input channel, 6 filters, 5x5 kernel
        self.conv2 = nn.Conv2d(6, 16, 5)
        
        # Fully connected layers
        self.fc1 = nn.Linear(16 * 5 * 5, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 10)       # 10 output classes

    def forward(self, x):
        # Conv → ReLU → MaxPool
        x = F.max_pool2d(F.relu(self.conv1(x)), (2, 2))
        x = F.max_pool2d(F.relu(self.conv2(x)), 2)
        
        # Flatten for FC layers
        x = torch.flatten(x, 1)
        
        # Fully connected
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x

net = Net()
print(net)
```

---

### 3.2 Inspecting Parameters

```python
params = list(net.parameters())
print(len(params))            # number of parameter tensors
print(params[0].size())       # size of first layer's weights
```

---

### 3.3 Forward Pass

```python
input = torch.randn(1, 1, 32, 32)   # batch=1, channels=1, H=32, W=32
output = net(input)                  # calls net.forward() internally
print(output)
```

---

### 3.4 Loss Function

```python
criterion = nn.MSELoss()

target = torch.randn(10)            # dummy target
target = target.view(1, -1)         # reshape to match output

loss = criterion(output, target)
print(loss)
```

**Common loss functions:**

| Task | Loss Function |
|------|--------------|
| Regression | `nn.MSELoss()` |
| Binary classification | `nn.BCELoss()` |
| Multi-class classification | `nn.CrossEntropyLoss()` |

---

### 3.5 Backpropagation

```python
net.zero_grad()       # ⚠️ Always zero gradients before backward — else they accumulate!
loss.backward()       # compute gradients
print(net.conv1.bias.grad)  # check gradient of a layer
```

---

### 3.6 Weight Update (Optimizer)

```python
import torch.optim as optim

optimizer = optim.SGD(net.parameters(), lr=0.01)

# Training step
optimizer.zero_grad()          # zero gradients
output = net(input)            # forward pass
loss = criterion(output, target) # compute loss
loss.backward()                # backward pass
optimizer.step()               # update weights
```

**Common optimizers:**

| Optimizer | When to use |
|-----------|------------|
| `SGD` | Simple, good baseline |
| `Adam` | Most common for DL, adapts learning rate |
| `RMSprop` | Good for RNNs |

---

## Part 4 — Training a Classifier

### The Full Pipeline

Training on real image data (CIFAR-10: 10 classes, 32×32 color images).

---

### 4.1 Load and Normalize Data

```python
import torch
import torchvision
import torchvision.transforms as transforms

# Transform: convert PIL image to tensor, normalize to [-1, 1]
transform = transforms.Compose([
    transforms.ToTensor(),
    transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))
])

# Download and load CIFAR-10
trainset = torchvision.datasets.CIFAR10(
    root='./data', train=True, download=True, transform=transform
)
trainloader = torch.utils.data.DataLoader(
    trainset, batch_size=4, shuffle=True, num_workers=2
)

testset = torchvision.datasets.CIFAR10(
    root='./data', train=False, download=True, transform=transform
)
testloader = torch.utils.data.DataLoader(
    testset, batch_size=4, shuffle=False, num_workers=2
)

classes = ('plane', 'car', 'bird', 'cat', 'deer',
           'dog', 'frog', 'horse', 'ship', 'truck')
```

---

### 4.2 Define the Network

```python
import torch.nn as nn
import torch.nn.functional as F

class Net(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = nn.Conv2d(3, 6, 5)    # 3 input channels (RGB)
        self.pool  = nn.MaxPool2d(2, 2)
        self.conv2 = nn.Conv2d(6, 16, 5)
        self.fc1   = nn.Linear(16 * 5 * 5, 120)
        self.fc2   = nn.Linear(120, 84)
        self.fc3   = nn.Linear(84, 10)

    def forward(self, x):
        x = self.pool(F.relu(self.conv1(x)))
        x = self.pool(F.relu(self.conv2(x)))
        x = torch.flatten(x, 1)
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x

net = Net()
```

---

### 4.3 Loss + Optimizer

```python
import torch.optim as optim

criterion = nn.CrossEntropyLoss()
optimizer = optim.SGD(net.parameters(), lr=0.001, momentum=0.9)
```

---

### 4.4 Training Loop

```python
for epoch in range(2):                    # 2 passes over the dataset
    running_loss = 0.0
    
    for i, data in enumerate(trainloader, 0):
        inputs, labels = data             # get batch
        
        optimizer.zero_grad()             # zero gradients ← IMPORTANT
        
        outputs = net(inputs)             # forward pass
        loss = criterion(outputs, labels) # compute loss
        loss.backward()                   # backward pass
        optimizer.step()                  # update weights
        
        running_loss += loss.item()
        
        if i % 2000 == 1999:              # print every 2000 batches
            print(f'[epoch {epoch+1}, batch {i+1}] loss: {running_loss / 2000:.3f}')
            running_loss = 0.0

print('Training finished!')
```

---

### 4.5 Save and Load Model

```python
# Save
torch.save(net.state_dict(), './cifar_net.pth')

# Load
net = Net()
net.load_state_dict(torch.load('./cifar_net.pth'))
```

---

### 4.6 Testing / Evaluation

```python
correct = 0
total   = 0

with torch.no_grad():                    # no gradients needed during eval
    for data in testloader:
        images, labels = data
        outputs = net(images)
        
        _, predicted = torch.max(outputs, 1)   # get class with highest score
        total   += labels.size(0)
        correct += (predicted == labels).sum().item()

print(f'Accuracy on 10,000 test images: {100 * correct / total:.1f}%')
```

**Per-class accuracy:**
```python
correct_pred = {classname: 0 for classname in classes}
total_pred   = {classname: 0 for classname in classes}

with torch.no_grad():
    for data in testloader:
        images, labels = data
        outputs = net(images)
        _, predictions = torch.max(outputs, 1)
        
        for label, prediction in zip(labels, predictions):
            if label == prediction:
                correct_pred[classes[label]] += 1
            total_pred[classes[label]] += 1

for classname, correct_count in correct_pred.items():
    accuracy = 100 * correct_count / total_pred[classname]
    print(f'Accuracy for {classname:5s}: {accuracy:.1f}%')
```

---

### 4.7 Training on GPU

```python
device = torch.device('cuda:0' if torch.cuda.is_available() else 'cpu')
print(device)

# Move model to GPU
net.to(device)

# Move data to GPU inside training loop
inputs, labels = data[0].to(device), data[1].to(device)
```

---

## Quick Reference Cheatsheet

### The Standard Training Loop (memorize this)

```python
for epoch in range(num_epochs):
    for inputs, labels in dataloader:
        
        optimizer.zero_grad()          # 1. Zero gradients
        outputs = model(inputs)        # 2. Forward pass
        loss = criterion(outputs, labels)  # 3. Compute loss
        loss.backward()                # 4. Backward pass
        optimizer.step()               # 5. Update weights
```

### Key Classes

| Class | Purpose |
|-------|---------|
| `torch.Tensor` | Core data structure |
| `nn.Module` | Base class for all models |
| `nn.Linear` | Fully connected layer |
| `nn.Conv2d` | 2D convolutional layer |
| `nn.CrossEntropyLoss` | Classification loss |
| `torch.optim.Adam` | Adaptive optimizer |
| `DataLoader` | Batches + shuffles dataset |

### Key Methods

| Method | Purpose |
|--------|---------|
| `.backward()` | Compute gradients |
| `.zero_grad()` | Clear old gradients |
| `.step()` | Update weights |
| `.to(device)` | Move to GPU/CPU |
| `.eval()` | Switch to eval mode (disables dropout, batchnorm updates) |
| `.train()` | Switch back to training mode |
| `torch.no_grad()` | Disable gradient tracking (use during eval/inference) |

---

## Connection to BCI / EEG Research

These PyTorch fundamentals directly map to what you'll do in the BCI research:

| Tutorial Concept | BCI Application |
|-----------------|-----------------|
| Tensors | EEG signals stored as tensors: `(batch, channels, timepoints)` |
| DataLoader | Loading EEG epochs from multiple subjects/sessions |
| Conv2d / Conv1d | Spatial-temporal feature extraction from EEG |
| Autograd + backprop | Training domain adaptation networks |
| `requires_grad=False` | Freezing feature extractor when adapting to new subject |
| CrossEntropyLoss | Motor imagery classification (left hand, right hand, etc.) |

