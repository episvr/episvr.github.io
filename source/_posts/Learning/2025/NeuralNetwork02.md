---
title: 深度学习小记(二) 
date: 2025-03-24
categories: 
  - Learning
tags: 
  - coding
mathjax: true
description: 在上一篇博客中，我们共同了解了神经网络的基本原理，这一次将聚焦于代码层次讨论如何“写”出一个神经网络。
---

## Part 01：异或拼接的判断机

### 主函数框架

```python
data = X
network.initialize()
for(epoches)
{
	network.forward(X)
	network.backward()
}
network.print()
```

### Forward

注意：

1. 保存所有的 L
2. 保存所有的 Z

```python
def forward(X):
	z_i = dot(L_i,w_i)
	L_{i+1} = sigmoid(z_i)
	return self.L_{final}
```

### Backward 

注意：

1. 计算所有的 delta：$\mathrm{d}C/ \mathrm{d}L$
2. 计算所有的 delta_error：$\mathrm{d}C/ \mathrm{d}z$

```python
def backward(X, y, learning_rate):
        # X -> A1 -> H -> A2 -> Y_hat
        # C = 1/2 (y-Yhat)^2 ; dC/dy = y - yhat
        output_error = y - self.Y_hat # dC/dy
        output_delta = output_error * sigmoid_derivative(self.Y_hat) #dC/dy * dY/dA2
        hidden_error = output_delta.dot(self.w2.T) # dA2/dH
        hidden_delta = hidden_error * sigmoid_derivative(self.H) #dA2/dH * dH/dA1

        self.w2 += self.H.T.dot(output_delta) * learning_rate #dC/dw2
        self.w1 += X.T.dot(hidden_delta) * learning_rate #dC/dw1
```

Q：是不是哪里不对？ `sigmoid_derivative(self.Y_hat)` 是不是应该是  `sigmoid_derivative(self.A2)`?

A：这是一个好问题！这与 `sigmoid_derivative` 的实现有关！

### Sigmoid

```python
# sigmoid激活函数
def sigmoid(x):
    return 1 / (1 + np.exp(-x))
# sigmoid的导数(?)
def sigmoid_derivative(y):
    return y * (1 - y)
```

注意其中的 `sigmoid_derivative`，它的输入并不是我们认为的自变量 `x`， 而是函数值 `y`。（即用 y 表示 y 的导数）。

> Sigmoid 激活函数：
>
> $\sigma(x)=\dfrac{1}{1+e^{-x}}$

### 把一切综合起来

```python
import numpy as np

def sigmoid(x):
    return 1 / (1 + np.exp(-x))
def sigmoid_derivative(x):
    return x * (1 - x)

class SimpleNeuralNetwork:
    def __init__(self, input_size, hidden_size, output_size):
        # 初始化权重 w1 和 w2
        self.w1 = np.random.rand(input_size, hidden_size)  # 输入层到隐藏层的权重
        self.w2 = np.random.rand(hidden_size, output_size)  # 隐藏层到输出层的权重
        
    def forward(self, X):
        # 前向传播
        # A1 = sig(A0 * w1)
        # A2 = sig(A1 * w2)
        self.A1 = np.dot(X, self.w1)  # 输入层到隐藏层的加权输入
        self.H = sigmoid(self.A1)  # 隐藏层输出
        self.A2 = np.dot(self.H, self.w2)  # 隐藏层到输出层的加权输入
        self.Y_hat = sigmoid(self.A2)  # 输出层输出
        return self.Y_hat

    def backward(self, X, y, learning_rate):
        # X -> A1 -> H -> A2 -> Y_hat
        # C = 1/2 (y-Yhat)^2 ; dC/dy = y - yhat
        output_error = y - self.Y_hat # dC/dy
        output_delta = output_error * sigmoid_derivative(self.Y_hat) #dC/dy * dY/dA2
        hidden_error = output_delta.dot(self.w2.T) # dA2/dH
        hidden_delta = hidden_error * sigmoid_derivative(self.H) #dA2/dH * dH/dA1

        self.w2 += self.H.T.dot(output_delta) * learning_rate #dC/dw2
        self.w1 += X.T.dot(hidden_delta) * learning_rate #dC/dw1
        
if __name__ == "__main__":
    # 输入数据（4个样本，每个样本2个特征）
    X = np.array([[0, 0],
                  [0, 1],
                  [1, 0],
                  [1, 1]])
    # 目标输出（XOR问题）
    y = np.array([[0], [1], [1], [0]])
    # 创建神经网络
    nn = SimpleNeuralNetwork(input_size=2, hidden_size=3, output_size=1)
    # 训练网络
    epochs = 10000
    learning_rate = 0.5
    for epoch in range(epochs):
        nn.forward(X)
        nn.backward(X, y, learning_rate)
    # 测试结果
    print("Final Output after training:")
    print(nn.forward(X))
```

## Part 02：封装结构的掉包侠

你一定觉得这么写很麻烦，尤其是反向传播的导数部分，我也是这么觉得的！我们可以借助pytorch中提供的工具！

| PyTorch API      | 作用                     |
| ---------------- | ------------------------ |
| `torch.tensor()` | 创建张量                 |
| `nn.Linear()`    | 定义全连接层             |
| `nn.Sigmoid()`   | 激活函数                 |
| `forward()`      | 定义前向传播             |
| `nn.MSELoss()`   | 计算损失                 |
| `optim.SGD()`    | 梯度更新（随机梯度下降） |
| `zero_grad()`    | 清空梯度                 |
| `backward()`     | 反向传播计算梯度         |
| `step()`         | 更新权重                 |

```py
import torch
import torch.nn as nn
import torch.optim as optim
import numpy as np

# 输入数据（XOR问题）
X = torch.tensor([[0, 0], [0, 1], [1, 0], [1, 1]], dtype=torch.float32)
y = torch.tensor([[0], [1], [1], [0]], dtype=torch.float32)

# 定义神经网络
class SimpleNN(nn.Module):
    def __init__(self):
        super(SimpleNN, self).__init__()
        self.hidden = nn.Linear(2, 3)
        self.output = nn.Linear(3, 1)
        self.sigmoid = nn.Sigmoid()

    def forward(self, x):
        x = self.sigmoid(self.hidden(x))
        x = self.sigmoid(self.output(x))
        return x

# 创建模型、损失函数和优化器
model = SimpleNN()
criterion = nn.MSELoss()
optimizer = optim.SGD(model.parameters(), lr=0.5)

# 训练网络
epochs = 10000
for epoch in range(epochs):
    optimizer.zero_grad()
    output = model(X)
    loss = criterion(output, y)
    loss.backward()
    optimizer.step()

# 测试结果
print("Final Output after training:")
print(model(X).detach().numpy())
```
是不是非常简单！（虽然造轮子也很有趣就是了）

定义模型和前向传播函数，创建损失函数和优化器，最后根据模板写训练，这三板斧就是绝大多数前向神经网络的写法了，接下来我们用轮子来写一个CNN试试。

### 定义模型

卷积神经网络利用卷积层（***conv***）提取局部特征，这一操作会缩小图片大小故通常会在外面补齐 padding

而卷积后使用最大池化层（***Max pool***）来在保留特征基础上缩小参数量。

```python
class CNN(nn.Module):
    def __init__(self):
        super(CNN, self).__init__()
        self.conv1 = nn.Conv2d(1, 32, kernel_size=3, padding=1)
        self.conv2 = nn.Conv2d(32, 64, kernel_size=3, padding=1)
        self.pool = nn.MaxPool2d(2, 2)
        self.fc1 = nn.Linear(64 * 7 * 7, 128)
        self.fc2 = nn.Linear(128, 10)
        self.relu = nn.ReLU()

    def forward(self, x):
        x = self.pool(self.relu(self.conv1(x)))
        x = self.pool(self.relu(self.conv2(x)))
        x = x.view(-1, 64 * 7 * 7) # 展平
        x = self.relu(self.fc1(x))
        x = self.fc2(x)
        return x
```

### 创建损失函数和优化器

数字识别属于分类任务，因此我们采取交叉熵损失函数。

优化使用 Adam（自适应梯度)，它能够对每个不同的参数调整不同的学习率

```python
criterion = nn.CrossEntropyLoss()
optimizer = optim.Adam(model.parameters(), lr=0.001)
```

### 训练

```python
epochs = 10
for epoch in range(epochs):
    running_loss = 0.0
    for batch_idx, (images, labels) in enumerate(trainloader):
        images, labels = images.to(device), labels.to(device)
        optimizer.zero_grad()
        outputs = model(images)
        loss = criterion(outputs, labels)
        loss.backward()
        optimizer.step()
        running_loss += loss.item()
```

### 加一点点细节

```python
import torch
import torch.nn as nn
import torch.optim as optim
import torchvision
import torchvision.transforms as transforms

# 1️⃣ 载入 MNIST 数据集（从 .data 目录）
transform = transforms.Compose([transforms.ToTensor()])
trainset = torchvision.datasets.MNIST(root='./data', train=True, download=False, transform=transform)
trainloader = torch.utils.data.DataLoader(trainset, batch_size=64, shuffle=True)

# 2️⃣ 定义 CNN 模型
class CNN(nn.Module):
    def __init__(self):
        super(CNN, self).__init__()
        self.conv1 = nn.Conv2d(1, 32, kernel_size=3, padding=1)
        self.conv2 = nn.Conv2d(32, 64, kernel_size=3, padding=1)
        self.pool = nn.MaxPool2d(2, 2)
        self.fc1 = nn.Linear(64 * 7 * 7, 128)
        self.fc2 = nn.Linear(128, 10)
        self.relu = nn.ReLU()

    def forward(self, x):
        x = self.pool(self.relu(self.conv1(x)))
        x = self.pool(self.relu(self.conv2(x)))
        x = x.view(-1, 64 * 7 * 7)
        x = self.relu(self.fc1(x))
        x = self.fc2(x)
        return x

# 3️⃣ 选取损失函数和优化器
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
model = CNN().to(device)

criterion = nn.CrossEntropyLoss()
optimizer = optim.Adam(model.parameters(), lr=0.001)
scheduler = optim.lr_scheduler.StepLR(optimizer, step_size=5, gamma=0.5) # 每5轮学习率减半
# 4️⃣ 训练 CNN
epochs = 10
for epoch in range(epochs):
    running_loss = 0.0
    for batch_idx, (images, labels) in enumerate(trainloader):
        images, labels = images.to(device), labels.to(device)

        optimizer.zero_grad()
        outputs = model(images)
        loss = criterion(outputs, labels)
        loss.backward()
        optimizer.step()

        running_loss += loss.item()
        if batch_idx % 100 == 0:  # 每 100 个 batch 打印一次损失
            print(f"Epoch [{epoch+1}/{epochs}], Batch [{batch_idx}/{len(trainloader)}], Loss: {loss.item():.4f}")

    scheduler.step()  # 更新学习率
    print(f"🔹 Epoch {epoch+1} 完成，当前学习率: {scheduler.get_last_lr()[0]:.6f}, 平均损失: {running_loss/len(trainloader):.4f}")

print("✅ CNN 训练完成！🎉")
```

总之就是：

> ***定义模型后***
> ***损失函数优化器***
> ***最后写训练***



------

感谢你看到这里，天气也是热起来了，今天要放的歌是…《黎明与萤火》

<div class="lyrics-container">
    <!-- 歌曲信息 -->
    <div class="song-info">
        <p class="song-title">
            <svg class="w-6 h-6 text-gray-800 dark:text-white" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor" viewBox="0 0 24 24">
                <path d="M18.6216 3.21667c.2391.1897.3784.47817.3784.78334V15.6667c0 .0412-.0025.0818-.0073.1218.0048.0698.0073.1404.0073.2115 0 1.6569-1.3431 3-3 3s-3-1.3431-3-3 1.3431-3 3-3c.3506 0 .6872.0602 1 .1707V9.2602l-8 1.8667V18l-.00001.0032C8.99824 19.6586 7.65577 21 6 21c-1.65685 0-3-1.3431-3-3s1.34315-3 3-3c.35064 0 .68722.0602 1 .1707V6.33334c0-.46474.32018-.86823.77277-.97384l9.99953-2.33321c.1486-.03477.3012-.03465.4467-.00201.1427.03202.2783.09532.3964.18752.0021.00162.0041.00324.0062.00487Z" />
            </svg>
            夜明けと蛍
        </p>
    </div>
    <div class="lyrics-display">
        <div class="lyric-item">
            <div class="original-lyric">形のない歌で朝を描いたまま</div>
            <div class="translated-lyric">仍是以无形的歌声　去幻想着清晨</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">浅い浅い夏の向こうに</div>
            <div class="translated-lyric">于那浅浅的　浅浅的　夏日的彼方</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">冷たくない君の手のひらが見えた</div>
            <div class="translated-lyric">我并不寒冷　因为能看见你的手心</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">淡い空　明けの蛍</div>
            <div class="translated-lyric">淡色天空中　有着黎明的萤火</div>
        </div>
    </div>
</div>

感谢你的阅读~
