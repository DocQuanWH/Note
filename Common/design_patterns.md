# 设计模式

## 1. 设计模式设计原则
### 1.1 单一职责原则（SRP）
接口一定要做到单一职责，类的设计尽量做到**只有一个原因或因素引起变化**。

### 1.2 里氏替换原则（LSP）
继承是非常优秀的面向对象语言机制，有诸多优点：**提高代码的重用性、可扩展性（开放性）**。但继承也有很多缺点，如子类必须继承所有基类的方法，增强了代码的耦合性等。**里氏替换原则**就是一个用来规范继承的原则，从而使之利大于弊，其定义如下：
> **引用基类的任何地方都可以替换成子类，而不引起任何错误或异常。**

**里氏替换原则**为继承定义了一个良好的规范，上述的定义包含以下3层含义：
* **子类必须完全实现基类的方法**。
* **在设计函数接口时，通常会将抽象类（基类）作为函数参数传入，如果不能这样做，说明已经违背了LSP**。
* **如果子类不能完整的实现基类的方法，说明父类的某些方法已经在父类发生“畸变”，此时应该将当前继承关系断开**。

如果不符合**LSP**原则，建议避免使用继承或则重新设计继承结构。

### 1.3 依赖导致原则

### 1.4 开闭原则

### 1.5 迪米特法则

### 1.6 接口隔离原则

## 2. 单例模式
单例模式定义：**确保类只有一个实例，而且自行实例化并向整个系统提供这个实例**，能有效的减少内存和性能开销。
### 恶汉式
主动创建实例，线程安全，不需要加锁。
```C++
class Singleton
{
public:
	static Singleton* getInstance() { return m_Instance; }
protected:
	Singleton() {}
private:
	static Singleton* m_Instance;
};

Singleton* Singleton::m_Instance = new Singleton();
```
### 懒汉式
被动创建，线程不安全，需要加锁。
```C++
class Singleton
{
public:
	static Singleton* getInstance() 
	{ 
		if (m_Instance == NULL) { m_Instance = new Singleton(); }
		return m_Instance; 
	}
protected:
	Singleton() {}
private:
	static Singleton* m_Instance;
};

Singleton* Singleton::m_Instance = NULL;
```