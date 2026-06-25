- torch.autograd automatically calculates derivatives (gradients) 
- gradients are used to train neural networks 
- neural networks are functions that are executed on some input data. These functions are defined by parameters (weights and biases) which are stored as tensors
- During training, a neural network makes predictions. If the prediction is wrong, it computes an error. Then it needs to know: "How should each weight change to reduce this error?" That information comes from the gradient. This is why gradients are important in nn training 
- nn training process happens in 2 steps:
   1. forward propagation: nn makes prediction by running input data through its functions
   2. backward propagation: collect derivatives of error wrt to parameters, nn adjusts its parameters proportionate to the gradient descent
- torchvision → tools and pretrained vision models
- ResNet-18, a popular convolutional neural network. It knows how to recognize 1000 categories of objects
- model = resnet18(weights=ResNet18_Weights.DEFAULT) this loads resnet-18
- data = torch.rand(1, 3, 64, 64)  : this creates a random image tensor 
1 → batch size (one image)
3 → color channels (RGB)
64 → height
64 → width
- labels = torch.rand(1, 1000) : corresponding label
- prediction = model(data) # forward pass
loss = (prediction - labels).sum()
loss.backward() # backward pass
- The optimizer updates the parameters to reduce that error
- Stochastic Gradient Descent is one of the simplest and most important optimizers.
- this generates the optimizer : 
optim = torch.optim.SGD(model.parameters(), lr=1e-2, momentum=0.9)
- lr meaning learning rate it controls how big each update step is. momentum helps the optimizer move smoothly
-  optim.step() #gradient descent : initiation
-  a = torch.tensor([2., 3.], requires_grad=True) : here requires_gard = True tells the computer to keep tracking this tensor as it will require gradients later 
 
Vector calculus using autograd:
- pytorch does not calculate the jacobian as it can be huge. Instead it uses vector jacobian product
- vector jacobian product: Jᵀ · v 

Computational Graphs 
- whenever we perform operations on tensors with requires_grad=True, PyTorch automatically builds a computational graph.
- It is a Directed Acyclic Graph (DAG).
- DAG has two aspects: leaves (which are the input tensors) and roots (which are the output tensors)
- when we run a model, two things happen in forward pass: running the operation to compute a result tensor and then assigning a .grad_fn to it.
- The backward pass kicks off when .backward() is called
- three things happen in backward pass: gradients are calculated, gradients are stored in tensor.grad and then finally the chain rule is propagated
- x.grad (contains dy/dx)
- If a variable depends on another through multiple steps, chain rule needs to be applied. autograd does this automatically. 
   for eg: z = y * 3    y = x * 2
   then dz/dx = dz/dy × dy/dx = 3 × 2 = 6
- PowBackward() , MulBackward(), SubBackward() are different operations

Exclusion from DAG
-Autograd (in PyTorch) only tracks operations if gradients are needed.
- requires_grad = True only then gradients are calculated
-If ANY input requires gradients → output also requires gradients
- If NO inputs require gradients → output does NOT require gradients
-In a NN, parameters that don’t compute gradients are usually called frozen parameters
- fine tuning is the process in which u freeze most of the model and only train the classifier layer
- for param in model.parameters():
  param.requires_grad = False
  model.fc = nn.Linear(512, 10) 
Now all the parameters except model.fc are frozen