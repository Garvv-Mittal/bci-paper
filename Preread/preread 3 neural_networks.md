- torch.nn package makes neural networks, works on top of autograd
- it takes input, feeds it through dufferent layers and then generates an output
- standard training loop of a neurak network:
  1. building a model which contains wieghts 
  2. iterate over a dataset (not training a data set all at once for better efficiency)
  3. process input through layers
  4. compute loss
  5. propagate gradients
  6. update weights according to the gradients (gradient descent)

CNN (Complete Convolutional Neural Network)
- It uses convolution operations to automatically learn patterns from data.
- specially designed to work on grid like data and images 
- core is convolution operation
- flow: Image → Convolutions → Pooling → Flatten → Fully Connected → Output
- convolution layers extract features like edges,textures, shapes
- self.conv1 = nn.Conv2d(1, 6, 5) # example of  convulation layer
- linear layers convert extracted features to final predictions
-   c3 = F.relu(self.conv2(s2)) # uses ReLu activation function
- activation fucntions like ReLu add non linearity for learning complex data
- Pooling is a downsampling operation used to reduce the size of feature maps
- FC layers are the final layers of a neural network that make decisions.
- net.parameters() returns weights and biases
- torch.nn only supports mini-batches

Loss Function
- A loss function takes the (output, target) pair of inputs, and computes a value that estimates how far away the output is from the target.
- MSE loss (mean squared error) : criterion = nn.MSELoss()
- take difference between prediction and target, square it then average it

Backprop
- net.zero_grad()     # zeroes the gradient buffers of all parameters, wihtout this gradients will be accumulated to existing gradients

Update the weights
- Stochastic Gradient Descent (SGD) is one of the simplest and most widely used optimization algorithms for training neural networks.
- weight = weight - learning_rate * gradient
- After computing gradients using backpropagation, SGD updates each weight 
- SGD, Nesterov-SGD, Adam, RMSProp these are different updation rules 
- torch.optim is the module that provides optimization algorithms used to update model parameters (weights) during training.