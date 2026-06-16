Neural networks are constructed using the torch.nn package. A typical training procedure for a neural network involves the following steps:



\* Define the neural network architecture with learnable parameters (weights and biases).

\* Iterate over a dataset of inputs.

\* Process input through the network (Forward pass).

\* Compute the loss (how far the output is from being correct).

\* Propagate gradients back into the network parameters (Backward pass).

\* Update the weights of the network, typically using a simple update rule: weight = weight - learning\_rate \* gradient.



\---



1\. Defining the Network



In PyTorch, you define a neural network by subclassing nn.Module. You must write two essential methods:



\* \*\*init\*\*: Where you define the layers of the network (e.g., convolutional layers, linear layers).

\* forward: Where you define the path data takes through the layers. The backward method (to calculate gradients) is automatically created for you using Autograd.



Here is a clean implementation of a simple convolutional neural network (CNN) used for processing images:



import torch

import torch.nn as nn

import torch.nn.functional as F



class Net(nn.Module):



```

def \_\_init\_\_(self):

&#x20;   super(Net, self).\_\_init\_\_()

&#x20;   # 1 input image channel, 6 output channels, 5x5 square convolution

&#x20;   self.conv1 = nn.Conv2d(1, 6, 5)

&#x20;   self.conv2 = nn.Conv2d(6, 16, 5)

&#x20;   # an affine operation: y = Wx + b

&#x20;   self.fc1 = nn.Linear(16 \* 5 \* 5, 120)  # 5x5 from image dimension

&#x20;   self.fc2 = nn.Linear(120, 84)

&#x20;   self.fc3 = nn.Linear(84, 10)



def forward(self, x):

&#x20;   # Max pooling over a (2, 2) window

&#x20;   x = F.max\_pool2d(F.relu(self.conv1(x)), (2, 2))

&#x20;   # If the size is a square, you can specify a single number

&#x20;   x = F.max\_pool2d(F.relu(self.conv2(x)), 2)

&#x20;   x = torch.flatten(x, 1) # flatten all dimensions except the batch dimension

&#x20;   x = F.relu(self.fc1(x))

&#x20;   x = F.relu(self.fc2(x))

&#x20;   x = self.fc3(x)

&#x20;   return x



```



net = Net()

print(net)



\---



2\. Crucial Takeaways for Data Inputs



\* torch.nn only supports mini-batches: The entire torch.nn package only supports inputs that are a mini-batch of samples, rather than a single sample.

\* Handling a single sample: If you have a single sample, you must create a fake batch dimension using input.unsqueeze(0).

\* Expected dimensions: For example, nn.Conv2d expects a 4D Tensor of size (nSamples x nChannels x Height x Width).



To pass random data through the network:



input\_data = torch.randn(1, 1, 32, 32)

out = net(input\_data)

print(out)



\---



3\. Recapping Gradients \& Loss Functions



A loss function takes the (output, target) pair of inputs, and computes a value that estimates how far away the output is from the target. There are several different loss functions under the torch.nn package. A simple loss is nn.MSELoss which computes the mean-squared error between the input and the target.



target = torch.randn(10)  # a dummy target, for example

target = target.view(1, -1)  # make it the same shape as output

criterion = nn.MSELoss()



loss = criterion(out, target)

print(loss)



When you follow the loss backwards using the grad\_fn attribute, you can see the graph of computation:



print(loss.grad\_fn)  # MSELoss

print(loss.grad\_fn.next\_functions\[0]\[0])  # Linear layer

print(loss.grad\_fn.next\_functions\[0]\[0].next\_functions\[0]\[0])  # ReLU



\---



4\. Backpropagation



To backpropagate the error, all we have to do is call loss.backward(). You need to clear the existing gradients first, otherwise gradients will be accumulated to existing gradients.



net.zero\_grad()     # zeroes the gradient buffers of all parameters



print('conv1.bias.grad before backward')

print(net.conv1.bias.grad)



loss.backward()



print('conv1.bias.grad after backward')

print(net.conv1.bias.grad)



\---



5\. Updating the Weights



The simplest update rule used in practice is Stochastic Gradient Descent (SGD):



weight = weight - learning\_rate \* gradient



Instead of updating weights manually with standard python code, PyTorch features a dedicated package called torch.optim that implements all variants of optimizers (like SGD, Nesterov-SGD, Adam, RMSProp).



import torch.optim as optim



\# create your optimizer



optimizer = optim.SGD(net.parameters(), lr=0.01)



\# in your training loop:



optimizer.zero\_grad()   # zero the gradient buffers

output = net(input\_data)

loss = criterion(output, target)

loss.backward()

optimizer.step()    # Does the update based on the gradients



\---



**nn.Module: The base class for all neural network modules in PyTorch. It tracks layers and weights.**



**forward(x): The function where you outline the math operations and connections. Executed automatically when you call net(input).**



**loss.backward(): Finds the graph of functions from the loss calculation down to the leaf inputs and computes gradients.**



**optimizer.step(): Updates the values of parameters across layers using the calculated gradients and learning rate rules.**



**optimizer.zero\_grad(): Crucial function used at the start of a training loop iteration to clear historical gradient accumulations.**

