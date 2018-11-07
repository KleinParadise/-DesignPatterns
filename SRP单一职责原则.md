定义:应该有且仅有一个原因引起类的变更。
  
通过一个手机抽象类来描述手机的行为
```cpp
class Phone  
{  
public:  
    Phone(){};  
    virtual ~Phone(){};  
    virtual void Dial(string phoneNum)=0;//拨电话  
    virtual void Chat()=0;//通话
    virtual void Hangup()=0;//挂电话  
};
```
这个抽象类符合一般常规思维设计。但是违背了单一职责原则。他包含了两个职责:Dial(),Hangup()实现的是协议管理,Chat()实现是数据传输。这个两个职责都会引起实现
类的变化。并且这两个职责的变化不相互影响。按照单一职责原则应该把这个抽象类拆分为两个抽象类。其实现代码如下：

```cpp
class ConnectionManager{
  public:
  ConnectionManager(){};  
  virtual ~ConnectionManager(){};  
  virtual void Dial(string phoneNum)=0;//拨电话    
  virtual void Hangup()=0;//挂电话
}

class DataTransfer{
  public:
  DataTransfer(){};  
  virtual ~DataTransfer(){};  
  virtual void Chat()=0;//通话
}

class Phone :public ConnectionManager ,public DataTransfer 
{  
public:  
    Phone(){};  
    ~Phone(){};  
    virtual void Dial(string phoneNum){};//拨电话    
    virtual void Chat(){};
    virtual void Hangup{};//挂电话  
};
```
通过上述方式，在一个类中实现了两个接口,把两个职责融合在一个类中。即面向接口编程，对外公布的是接口而不是实现类。

单一职责原则带来好处：
  1. 类的复杂性降低,实现什么职责都有清晰明确的定义。
  2. 可读性提高
  3. 可维护性提高
  4. 变更引起的风险降低。一个接口的修改只对相应的实现类有影响,对其他接口无影响。
  
  
对于接口(抽象类)在设计的时候一定要做到单一。但是对于类的设计要根据具体的情况考量,否则按照单一原则生搬硬套会引起类的剧增,增加维护成本。同时也人为的增加了
类的复杂性。对于函数的设计，也适用单一职责原则。一个函数尽可能只做一件事情,做到函数职责清晰,单一。比较好的函数设计如下:
```cpp
class UserMgr  
{  
public:  
    UserMgr(){};  
    virtual ~UserMgr(){};  
    virtual void ChangeName(string newName)=0;
    virtual void ChangeAddress(string newAddress)=0;
    virtual void ChangeOfficeTel(string newTel)=0; 
};
```
