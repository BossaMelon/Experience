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
CrossEntropyLoss(input,target)集成了Softmax,其输入为input.shape=(data_num,class_num), target.shape=(data_num,). target为long型，范围是0-(class_num-1),在此不需要one-hot编码
https://pytorch.org/docs/stable/generated/torch.nn.CrossEntropyLoss.html#torch.nn.CrossEntropyLoss

BCELoss,适用于二分类，输出为单节点，input.shape=target.shape=(data_num,*)
https://pytorch.org/docs/stable/generated/torch.nn.BCELoss.html#torch.nn.BCELoss

BCEwithLogitLoss，结合了BCELoss和Sigmoid
M
MSELoss，必须input.shape=target.shape
https://pytorch.org/docs/stable/generated/torch.nn.MSELoss.html


### dreate tensor

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
