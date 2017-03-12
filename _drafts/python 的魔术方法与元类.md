# 关于 python 的魔术方法

## metaclass

按照 Python 官方文档上的介绍：
The class of a class. Class definitions create a class name, a class dictionary and a list of base calsses. The metaclass is responsible for taking those three arguments and creating the class. Most object oriented programming languages provides a default umplementation. What makes Python special is that it is possible to create custom metaclasses. Most user never need this tool, but when the need arises, metaclasses can procied powerful, elegant solutions. They have been used for logging attribute access, adding thread-safety, tracking object creation, implementing sigletons, and many other tasks.

也就是说， metaclass 是类的类。事实上，metaclass是类的生成器。我们知道生成器生成的是对象，所以，是的，类也是对象。
类是这样一种对象：其中包含了生成其它对象所需的蓝图，并且，该对象可以依照蓝图生成其它对象。
那么按照这个定义，metaclass也是对象。没错。

事实上，python 中的所有东西都是对象。

只要你使用关键词 class，当 Python 的解释器解释道此处，它将在内存中生成一个对象，这个对象可以用来生成其它对象。Python 中能够用在对象上的操作，在类上也是全部可用的（当然，因为类也是对象）。
包括：
1. 将它赋予一个变量；
2. 可以被拷贝；
3. 可以为其绑定属性；
4. 可以作为参数传递。

因为类也是对象，所以可以动态的创建类，就像创建一个对象一般，只需要使用关键字 class。
同样的，对象不能自己产生自己，必须有个什么东西产生对象。既然类可以被产生，那么一定存在一个产生类的对象。

## type
一般的我们使用以 type(obj) 的方式使用 type，此时它将返回 obj 的类对象。

但是 type 还具有一种完全不同的用法： type(name, parents, attrs)

以这种方式调用 type 将会返回一个全新的类。该类以 name 命名，继承子 parents 元组中给出的类，同时绑定了 attrs 字典中的属性。

甚至，你可以为动态生成的类添加方法。只需要提供一个签名合适的方法即可
* def method(self):pass

到现在我们就明白了，为什么类的方法需要参数 self。事实上，参数self并没有任何特殊的含义，只是一个约定俗称的名称。在对象上以点运算符调用方法时，会隐式的将对象作为第一个对象传递给要调用的方法。除此之外，对象的方法和普通的方法没有任何区别。

***

所以，type 就是一个 metaclass。它是用来生成类对象的对象。

当我们生成一个类的时候，将自动的查找这个类指定的 metaclass，然后使用指定的metaclass来生成对象。默认情况下，metaclass就是 type。

虽然，metaclass的名字中含有 class，但是它并不一定是个类。原因很清楚，metaclass所指定的对象将在类创建时被调用。所以metaclass只要是一个可调用对象即可。比如函数。

metaclass所做的，不过是三点：

1. 拦截类的创建请求

2. 修改类

3. 返回修改之后的类

这里需要注意，操作的对象都是类对象。

