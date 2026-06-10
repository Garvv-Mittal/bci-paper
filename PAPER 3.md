# PyTorch

A tensor is the primary data structure in PyTorch used to store or manipulate numerical data.they are similar to Numpy array but can be accelerated using gpu.

tensors are used to store input/output and model parameter , support automatic differentiation ,can run on cpu and gpu.

Tensor Initialisation 

(from python data)—>

bci = [ [3,4],[5,6]]

x =torch.tensor(bci)

from numpy to array)—>

arr= np.array(bci)

x=torch.from_numpy(arr)

(from existing tensor) —>

x_ones = torch.ones_like(x)
x_rand = torch.rand_like(x, dtype=torch.float)

TENSOR ATTRIBUTES-

tensor.shape ———→ shape (dimension of tensor)
tensor.dtype ————> dtype (datatype) 
tensor.device ————> device (cpu/gpu storage location)

tensor = [tensor.to](http://tensor.to/)("cuda") ————→ moves tensor from cpu to gpu for faster computation.

torch.cat() ——> join tensor along an existing dimensions 

torch.stack() ———> create new dimension while joining tensor.

tensor multiplication - element wise multiplication ———> tensor*tensor or tensor.mul(tensor)

matrix multiplication - tensor.matmul(tensor.T) or tensor @ tensor.T Performs linear algebra matrix multiplication.

Numpy to tensor conversion  <———>

 n = tensor.numpy() tensor to numpy

t = torch.from_numpy(n) numpy to tensor.

# auto grad

torch.autograd is PyTorch's automatic differentiation engine.

It automatically calculates gradients, which are needed for training neural networks.

Main purpose-Calculate derivatives automatically,Help update model parameters.Used during backpropagation(process by which neural network learns from its mistake)

How Neural Network Training Works :-

1.forward propagation-Input data is passed through the network.——>Each layer performs some computation.——>Model produces a prediction.

prediction = model(data)

2.backward pass-Model prediction may be wrong,We calculate the error (loss),Then autograd calculates gradients,These gradients are used to improve the model.

flow-

prediction = model(data) —— loss = (prediction - labels).sum() —— loss.backward() —— 
optim.step()

Autograd tracks operations only for tensors with: requires_grad=true

COMPUTAIONAL GRAPH 

PyTorch creates a computational graph during the forward pass.The graph stores:Tensors ,Operations performed on tensors

During the backward pass, gradients are computed using this graph.

dynamic graph-

The computational graph is recreated after every iteration.

This allows changes in operations, tensor sizes, and control flow during execution.

torch.no_grad()

Used when gradients are not required.Common uses: Model evaluation,Testing,Inference.Benefits: Faster execution,Lower memory usag.

PyTorch provides the  torch.nm package for building neural networks. A neural network consists of multiple layers connected together to process input data and generate predictions.The learnable components of a neural network are called **parameters.**

## Neural Network Training Workflow

neural network training workflow-  Defining the network architecture.——Passing input data through the network.——Generating predictions.——Computing the loss.———Calculating gradients using backpropagation.——-Updating model parameters.——Repeating the process for multiple iterations.

model parameters-

All learnable weights and biases are stored as model parameters.PyTorch automatically registers these parameters when layers are defined inside an nn, module These parameters are updated during training.

input format

The example network expects image data in batch form.

Input dimensions follow: batch size * channels * height 8 width

loss function

A loss function measures the difference between the predicted output and the actual target. A lower loss value indicates better model performance.

Backpropogation 

After calculating the loss, gradients are computed using backpropagation.

optimiser

An optimizer updates the network parameters using the computed gradients.

gradient management 

Gradients accumulate in PyTorch. Therefore, gradients should be cleared before each training iteration to prevent unwanted accumulation.

# TRAINING A CLASSIFIER IN PyTorch

After defining a neural network, the next step is to train it using data. For image classification, PyTorch provides the torch vision package, which contains:  Popular datasets ,Data loading utilities ,Image transformations

Main Steps for Training a Classifier

load dataset

Training and testing datasets are loaded separately. Before training: Images are converted into tensors.

define the neural network

The network used in this tutorial is a Convolutional Neural Network (CNN).

Main components: Convolution layers,ReLU activation,Max Pooling layers,Fully Connected layers

A loss function measures how different the prediction is from the actual label.

deine optimiser 

The optimizer updates model parameters using gradients.

train the network

During training: Input images are passed through the network,Predictions are generated,Loss is calculated,Gradients are computed using backpropagation.,Weights are updated by the optimizer. This process is repeated for multiple batches and epochs.

After training, the model parameters can be saved.

This allows the trained model to be reused later without retraining.

Training gpu

Training can be performed on a GPU instead of a CPU. because it provide  - Faster computation ,Faster training ,Better performance for larger models.