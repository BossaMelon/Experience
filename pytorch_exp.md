# Pytorch Experience

### data_type
torch模型默认为float32(float),格式所以最好将数据转换成float32,另外float32在cpu上表现也更好

label在当前版本的torch下需要用int64(long)，未来更新可能会加入对int的支持

### tensor dim
在cnn中需要吧dataset整理成[B,C,H,W]顺序，跟其他框架不同的是颜色通道的位置，可以通过x=np.transpose(x,(0,3,1,2))

### model mode
在训练中，train阶段需要model.train(),val阶段需要model.eval(),会影响dropout和batchnormalization等层的状态。

### about optimizer and auto_grad
每个Batch导入之后，需要首先将权重的梯度清零，optimizer.zeor_grad(),因为梯度计算默认为累加。

只有在train阶段需要追踪计算图，所以torch.set_grad_enabled(phase==’train’),在val阶段之需正向传播不需要反向传播，也就不需要grad了

### about loss:
#### CrossEntropyLoss
集成了Softmax,其输入为input.shape=(data_num,class_num), target.shape=(data_num,). target为long型，范围是0-(class_num-1),在此不需要one-hot编码
https://pytorch.org/docs/stable/generated/torch.nn.CrossEntropyLoss.html#torch.nn.CrossEntropyLoss

#### BCELoss
适用于二分类，输出为单节点，底数为e，input.shape=target.shape=(data_num,*)
https://pytorch.org/docs/stable/generated/torch.nn.BCELoss.html#torch.nn.BCELoss

$$
J(\theta)=-\frac{1}{m} \sum_{i=1}^{m}\left[y^{(i)} \log h\left(x^{(i)}, \theta\right)+\left(1-y^{(i)}\right) \log \left(1-h\left(x^{(i)}, \theta\right)\right)\right]
$$

$y^i$为第i个标签，$h(x^i,\theta)$ 为第i个预测

- 当 $y^i=1$ 时
  - $h(x^i,\theta)$ → 1， $\log h\left(x^{(i)}, \theta\right)$ → 0，$y^i\log h\left(x^{(i)}, \theta\right)$ → 0
  - $h(x^i,\theta)$ → 0， $\log h\left(x^{(i)}, \theta\right)$ → $-\infty$，$-y^i\log h\left(x^{(i)}, \theta\right)$ → $\infty$

- 当 $y^i=0$ 时
  - $h(x^i,\theta)$ → 1， $\log (1- h\left(x^{(i)}, \theta\right))$ → 0，$（1-y^i）\log （1-h\left(x^{(i)}, \theta\right))$ → 0
  - $h(x^i,\theta)$ → 0， $\log (1- h\left(x^{(i)}, \theta\right))$ → $-\infty$，$-(1-y^i)\log (1 - h\left(x^{(i)}, \theta\right))$ → $\infty$

最后Loss是Batch中每一个BCE的平均。总体来说，当预测值和真实值接近的时候，Loss接近0.不接近的时候，Loss接近正无穷。



#### BCEwithLogitLoss
结合了BCELoss和Sigmoid

#### MSELoss
必须input.shape=target.shape
https://pytorch.org/docs/stable/generated/torch.nn.MSELoss.html


### create tensor

- 总会从原数据copy
  - torch.Tensor
    Tensor的class名，一般不直接用
  - torch.tensor
    工厂方法，更灵活

- 如果不转换device的话，不需要copy
  - torch.from_numpy
    将numpy.ndarray转化为tensor
  - torch.as_tensor
    将array-like转化为tensor，包括list，tuple，ndarray等

### 计算图 Computational graph
计算图是一个DAG(有向无环图)，其中节点代表变量(张量、矩阵、标量等)，边缘代表一些数学运算。
网络输入数据和weight，bias都是leaf node，需要被
A computation graph is a DAG(directed acyclic graph) in which nodes represent variables (tensors, matrix, scalars, etc.) and edge represent some mathematical operations
- require_grad: 为True时我们将会记录tensor的运算过程并为自动求导做准备，为False时候不参与求导计算。如果一个运算的所有输入都是require_grad=False,那么他的结果自动require_grad=False,如果有需要梯度的，那么结果也需要梯度
- is_leaf: 用户创建的都为叶子节点，当require_grad=True,is_leaf=True时，backward会计算该变量的梯度

### Pipeline
```python
# initialize the optimizer with the parameters of the model to be trained
optimizer=torch.nn.Adam(model.parameters()）

# loss.backward() will accumulate the gradient, so for each batch we need to reset the gradient of each parameter to 0.
optimizer.zero_grad()

# Disabling gradient calculation is useful for inference, when you are sure
#     that you will not call Tensor.backward(). It will reduce memory
#     consumption for computations that would otherwise have `requires_grad=True`.
with torch.set_grad_enabled(phase == 'train'):
    # forward() will create a new computation graph in each new iteration if set_grad_enabled(True)
    outputs = model(inputs)
    loss = criterion(outputs.squeeze(), labels)

    # backward + optimize only in training phase
    if phase == 'train':
        # backward() will calculate the derivative save it to model.parameters.grads and then release
        # the computation graph, so we should not do backward() twice
        loss.backward()
        # step() will actually change the value of the parameters
        optimizer.step()
```
