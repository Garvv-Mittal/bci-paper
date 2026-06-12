## Workflow Steps

### 1. Load and Normalize Data
Using the `torchvision` package, the dataset is downloaded and preprocessed. The images are normalized from the range $[0, 1]$ to $[-1, 1]$.


import torch
import torchvision
import torchvision.transforms as transforms

# Data augmentation and normalization
transform = transforms.Compose([
    transforms.ToTensor(),
    transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))
])


trainset = torchvision.datasets.CIFAR10(root='./data', train=True, download=True, transform=transform)
trainloader = torch.utils.data.DataLoader(trainset, batch_size=4, shuffle=True, num_workers=2)

testset = torchvision.datasets.CIFAR10(root='./data', train=False, download=True, transform=transform)
testloader = torch.utils.data.DataLoader(testset, batch_size=4, shuffle=False, num_workers=2)

classes = ('plane', 'car', 'bird', 'cat', 'deer', 'dog', 'frog', 'horse', 'ship', 'truck')

2. Define a Convolutional Neural Network (CNN)
A standard CNN architecture is designed featuring 2 Convolutional layers (nn.Conv2d), Max Pooling (nn.MaxPool2d), and 3 Fully Connected layers (nn.Linear).

Python
import torch.nn as nn
import torch.nn.functional as F

class Net(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = nn.Conv2d(3, 6, 5) # 3 input channels (RGB), 6 output channels, 5x5 kernel
        self.pool = nn.MaxPool2d(2, 2)  # 2x2 kernel, stride 2
        self.conv2 = nn.Conv2d(6, 16, 5)
        self.fc1 = nn.Linear(16 * 5 * 5, 120) # Flattened input to 120 neurons
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 10)    # 10 output classes

    def forward(self, x):
        x = self.pool(F.relu(self.conv1(x)))
        x = self.pool(F.relu(self.conv2(x)))
        x = torch.flatten(x, 1) # Flatten all dimensions except the batch dimension
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x

net = Net()


3. Define a Loss Function and Optimizer
Loss Function: nn.CrossEntropyLoss() (Standard for multi-class classification tasks).

Optimizer: optim.SGD (Stochastic Gradient Descent) with momentum.

Python
import torch.optim as optim

criterion = nn.CrossEntropyLoss()
optimizer = optim.SGD(net.parameters(), lr=0.001, momentum=0.9)


4. Train the Network (Training Loop)
We loop over our data iterator, pass the inputs to the network (forward pass), compute the loss, backpropagate the gradients (backward pass), and update the weights. (The tutorial runs this for 2 Epochs).

Python
for epoch in range(2): # Loop over the dataset multiple times
    running_loss = 0.0
    for i, data in enumerate(trainloader, 0):
        inputs, labels = data

        # Zero the parameter gradients
        optimizer.zero_grad()

        # Forward + Backward + Optimize
        outputs = net(inputs)
        loss = criterion(outputs, labels)
        loss.backward()
        optimizer.step()

        # Print statistics
        running_loss += loss.item()
        if i % 2000 == 1999: # Print every 2000 mini-batches
            print(f'[{epoch + 1}, {i + 1:5d}] loss: {running_loss / 2000:.3f}')
            running_loss = 0.0

print('Finished Training')


5. Test the Network on Test Data
After training, the model's performance is evaluated using the unseen test dataset.

Save the Model: torch.save(net.state_dict(), './cifar_net.pth')

Overall Accuracy: Checked across the whole test set (A random guess yields ~10% accuracy since there are 10 classes).

Class-wise Accuracy: Tracked individually for each specific category (e.g., 'cat', 'ship') to see where the model performs best or struggles.

Training on GPU (Optional)
If you have a CUDA-capable GPU available, you can transfer your model and tensors to the GPU to accelerate processing speeds:

Python
device = torch.device('cuda:0' if torch.cuda.is_available() else 'cpu')

# Move the network to GPU
net.to(device)

# Inside the training loop, move inputs and labels to GPU:
inputs, labels = data[0].to(device), data[1].to(device)