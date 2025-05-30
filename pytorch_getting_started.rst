Getting Started with PyTorch
============================

Installing PyTorch
------------------

Run:

.. code-block:: shell

   pip install torch torchvision

Creating a Simple Neural Network
--------------------------------

.. code-block:: python

   import torch
   import torch.nn as nn
   import torch.optim as optim

   class Net(nn.Module):
       def __init__(self):
           super(Net, self).__init__()
           self.fc1 = nn.Linear(100, 64)
           self.fc2 = nn.Linear(64, 10)

       def forward(self, x):
           x = torch.relu(self.fc1(x))
           x = torch.softmax(self.fc2(x), dim=1)
           return x

Training Your Model
-------------------

.. code-block:: python

   model = Net()
   criterion = nn.CrossEntropyLoss()
   optimizer = optim.Adam(model.parameters())

   # Example training loop (simplified)
   for epoch in range(5):
       optimizer.zero_grad()
       outputs = model(inputs)
       loss = criterion(outputs, labels)
       loss.backward()
       optimizer.step()

Further Resources
-----------------
.. seealso::

   PyTorch Homepage: https://pytorch.org/
   PyTorch Documentation: https://pytorch.org/docs/stable/index.html