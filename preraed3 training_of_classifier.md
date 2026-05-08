Training an image classifier
- CIFAR10: training and test datasets
- CIFAR-10 images are RGB
- so it uses the same idea as before, just the code needs ot be adpatted for colour images
- the data is loaded using torchvision
- transforms.ToTensor() #Converts image → PyTorch tensor
- transforms.Normalize((0.5,...), (0.5,...)) # transform is used for different operations
- normalize makes training faster
- trainloader = torch.utils.data.DataLoader #splits dataset into batches 
- CrossEntropyLoss: most commonly used loss function for classification problems (like CIFAR-10).
- outputs = scores for each class, torch.max picks highest score. That index = predicted class
- Why some classes perform poorly
   1. visual similarity (cat vs dog vs deer)
   2. small image size (32×32)
   3. limited training (only 2 epochs)

Training on GPUs
- training on gpus is much faster than cpus
- 'cuda:0' → first GPU
- net.to(device) #moves all models form cpu to gpu
- all inputs are to be moved as well
- cpu is okay for small networks