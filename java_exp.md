## 关于Java的一些心得

### Enhanced For Loop
for (variable:collection){}  
collection需要实现Iterable接口

### 关于常用接口
1. collection
2. map

### 关于泛型
- public <E> void printArray(E[] inputArray)
- 定义泛型方法需要在返回值前面加<E>
- 泛型多用于容器类
- 使用泛型时候可以 Queue<Interger> = new ArrayDeque<>()
  也可以 var queue = new ArrayDeque<Interger>()
- 泛型类不可以是基本，但可以是基本类型的包装型，int -> Integer
- 声明泛型时引用时，如果左侧已经写明泛型类，右侧可以写空<>，左侧写var时，右侧需要写全泛型类


### 关于Queue接口
- 继承自Collection接口
- add() vs offer() 加入元素到队列尾端，对于有限容量队列，队列达到容量上限时，add()会抛出异常，offer()返回false
- poll() vs remove() 删除队列头元素，当队列为空，poll()返回null,remove()抛出异常。
- element() vs peek() 查询队列头元素，当队列为空，element()抛出异常，peek()返回null。

### 关于类变量初始化
- 基础类型作为类变量时，JVM都自动赋初始值，引用类型均为null
  - byte、short、int、long -> 0
  - float -> 0.0f
  - double -> 0.0d
  - char -> “/u0000”
  - boolean -> false
  - reference -> null

### 关于casting
PigSon -> PigMom

- 向上转型总是可以的：PigMom pig1 = new PigSon();
- 向下转型只能把指向子类对象的父类引用赋给子类，相当于将引用层次还原。
  - PigSon pig2 = new PigMom(); 报错
  - PigSon pig2 = (PigSon) new PigMom();报错
  - PigMom pig2 = new PigSon(); PigSon pig3 = (PigSon)pig2;可以编译
  
