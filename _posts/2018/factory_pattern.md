---
layout: default
title: Factory Method Pattern
---


# Factory Method Pattern

### 基本概念

现在对该系统进行修改，不再设计一个按钮工厂类来统一负责所有产品的创建，而是将具体按钮的创建过程交给专门的工厂子类去完成。

这种抽象化的结果使这种结构可以在不修改具体工厂类的情况下引进新的产品，如果出现新的按钮类型，只需要为这种新类型的按钮创建一个具体的工厂类就可以获得该新按钮的实例，这一特点无疑使得工厂方法模式具有超越简单工厂模式的优越性，更加符合“开闭原则”。

### 基本元素
1. Product：抽象产品
2. ConcreteProduct：具体产品
3. Factory：抽象工厂
4. ConcreteFactory：具体工厂

![FactoryMethod](../../_images/FactoryMethod.jpg)

图片来源:
https://github.com/me115/design_patterns/blob/master/_static/FactoryMethod.jpg

代码示例:
https://github.com/me115/design_patterns/tree/master/code/FactoryMethod

### 继承关系
```
/* Product.h */
class Product {
	...
	// Define product interface
	// All products could be used
	virtual void use();
}
```

```
/* ConcreteProduct.h */
#include "Product.h"
class ConcreteProduct : public Product {
    ...
    // Concrete product use logic
    virtual void use();
}
```

```
/* Factory.h */
#include "Product.h"
class Factory {
	...
	// Define factory interface
	// All factories should create products
	virtual Product* factoryMethod();
}
```

```
/* ConcreteFactory.h */
#include "Product.h"
#include "Factory.h"
class ConcreteFactory : public Factory {
	...
	// Create a ConcreteFactory ptr
	virtual Product* factoryMethod(); 
}
```

```
/* main.cpp */
int main(int argc, char *argv[]) {
	Factory * fc = new ConcreteFactory();
	Product * prod = fc->factoryMethod();
	prod->use();
	...
}
```


### 设计思路

工厂设计模式有什么用？ - 白乔的回答 - 知乎
https://www.zhihu.com/question/24843188/answer/48943088

> 1. 隐藏具体类名，很多类隐藏得很深的，而且可能会在后续版本换掉
> 2. 避免你辛苦的准备构造方法的参数
> 3. 这个工厂类可以被配置成其它类
> 4. 这个工厂对象可以被传递

**针对上面的例子，隐藏了ConcreteProduct类名，隐藏构造函数参数（其实都是默认的），方便修改ConcreteProduct**


工厂设计模式有什么用？ - 兔子先生的回答 - 知乎
https://www.zhihu.com/question/24843188/answer/29171607

> 最近突然明白。工厂类最大好处让是使用者和对象的创建分离。使用者再也不担心，对象是怎么创建的。用上接口，使用者更不需要知道对象是谁。> 


> 还有第二个好处，减少了客户端B的修改.
如果要扩展concrete product类型，只要在代码包A里再增加ClassA101,....,ClassA200和具体对应的工厂就好了，B则不需要做什么改动.
如果不用工厂方法，那B里面，修改的会非常多.

**减少客户端代码改动，把数据结构和初始化工作都放在工厂方法模式中，客户端使用修改更方便**

### 举个例子
语言：Golang
项目：k8s
版本：1.1 

https://github.com/kubernetes/kubernetes

简单工厂模式,按照名称AlgorithmProvider来创建具体算法的调度器
客户端：

```
/* cmd/kube-scheduler/app/server.go */
func (s *SchedulerServer) createConfig(configFactory *factory.ConfigFactory) (*scheduler.Config, error) {
	...
	_, err := factory.GetAlgorithmProvider(s.AlgorithmProvider)
	if err != nil {
		return nil, err
	}
	...
```

工厂类

```
/* plugin/pkg/scheduler/factory/plugin.go */
var (
	schedulerFactoryMutex sync.Mutex

	// maps that hold registered algorithm types
	fitPredicateMap      = make(map[string]FitPredicateFactory)
	priorityFunctionMap  = make(map[string]PriorityConfigFactory)
	algorithmProviderMap = make(map[string]AlgorithmProviderConfig)
)
func GetAlgorithmProvider(name string) (*AlgorithmProviderConfig, error) {
	schedulerFactoryMutex.Lock()
	defer schedulerFactoryMutex.Unlock()

	var provider AlgorithmProviderConfig
	provider, ok := algorithmProviderMap[name]
	if !ok {
		return nil, fmt.Errorf("plugin %q has not been registered", name)
	}

	return &provider, nil
}
```





