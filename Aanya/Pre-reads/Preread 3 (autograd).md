Neural networks (NNs) are a collection of nested functions that are executed on some input data. These functions are defined by parameters (consisting of weights and biases), which in PyTorch are stored in tensors.



Training a NN happens in two steps:



***Forward Propagation***: In forward prop, the NN makes its best guess about the correct output.



***Backward Propagation***: In backprop, the NN adjusts its parameters proportionate to the error in its guess. It does this by traversing backwards from the output, collecting the derivatives of the error with respect to the parameters of the functions (gradients), and optimizing the parameters using gradient descent.





\---------



When training a neural network, the most critical step is Backpropagation. In this step, we adjust the model's parameters (weights and biases) based on the gradient (derivative) of the loss function with respect to those parameters.



torch.autograd is PyTorch’s automatic differentiation engine. It automatically calculates all these gradients for you, saving you from doing complex calculus by hand.



\---



1\. How Autograd Tracks Operations (The Direct Acyclic Graph)



Autograd keeps a record of data (tensors) and all executed operations in a directed graph called a DAG (Directed Acyclic Graph).



\* Leaves (Input Tensors): The input tensors you create.

\* Roots (Output Tensors): The final output tensor (usually the loss value).

\* Tracing: By tracing this graph from roots to leaves, PyTorch can automatically compute gradients using the chain rule.



\---



2\. Setting Up Tensors for Autograd



By default, PyTorch does not track operations on tensors because tracking requires extra memory. To tell PyTorch to start tracking history for a tensor, you use the property requires\_grad=True.



import torch



\# Create a tensor and tell PyTorch to track its history



x = torch.ones(5, requires\_grad=True)

print(x)



The grad\_fn Attribute



When you perform operations on a tensor that has requires\_grad=True, the resulting tensor will automatically get a grad\_fn property. This property references a specific math function that knows how to compute the backward step.



y = x + 2

print(y)



\# Output will show something like: grad\_fn=



Because y was created as a result of an addition operation, it holds an AddBackward0 function to undo or calculate the derivative of that addition later.



\---



3\. Computing Gradients (Backpropagation)



To calculate the derivatives, you simply call .backward() on the final output tensor (the root of your graph). PyTorch then traverses backward through the graph, computes the gradients, and stores them in each input tensor's .grad attribute.



Let's look at a complete mathematical example:



import torch



\# 1. Define input parameters



x = torch.ones(2, 2, requires\_grad=True)



\# 2. Do some math operations



y = x + 2

z = y \* y \* 3

out = z.mean()



print(f"Final output: {out}")



\# 3. Trigger backpropagation



out.backward()



\# 4. Check the gradients of 'out' with respect to 'x'



print(f"Gradients on x:\\n{x.grad}")



Crucial Rules for .backward()



\* Gradients accumulate: PyTorch does not clear out old gradients automatically when you call .backward(). If you call it twice, it will add the new gradients to the old ones. In actual training loops, you must explicitly clear them out using an optimizer (optimizer.zero\_grad()).

\* Scalar vs Vector: You can normally only call .backward() on a scalar tensor (a single number, like a loss value). If your output is a vector (multiple numbers), you must pass a gradient argument matching its shape.



\---



4\. Stopping Autograd (Disabling Tracking)



During model Evaluation / Inference (when you are testing your model, not training it), you don't need to compute gradients. Turning off tracking saves massive amounts of memory and speeds up code execution.



There are three ways to stop PyTorch from tracking gradients:



A. Using the torch.no\_grad() Context Manager (Recommended)

This temporarily blocks gradient tracking for a block of code.



x = torch.ones(5, requires\_grad=True)



print(x.requires\_grad) # True



with torch.no\_grad():

y = x \* 2

print(y.requires\_grad) # False



B. Using .detach()

This creates a brand new copy of the tensor that shares the same data but cuts off the history entirely.



x = torch.ones(5, requires\_grad=True)

y = x.detach()



print(y.requires\_grad) # False



C. Changing the Flag In-Place

You can turn off tracking directly on an existing tensor.



x = torch.ones(5, requires\_grad=True)

x.requires\_grad\_(False) # The underscore means in-place



\---





**requires\_grad=True: Tells PyTorch to start tracking history and build a computational graph for this tensor.**



**grad\_fn: Stores the specific math operation (e.g., ) used to create this tensor.**



**.backward(): Computes the derivatives automatically and pushes them down to the input leaves.**



**.grad: The attribute where the calculated gradient values are actually stored.**



**with torch.no\_grad(): A block wrapper used during inference to pause tracking, saving memory and processing power.**

