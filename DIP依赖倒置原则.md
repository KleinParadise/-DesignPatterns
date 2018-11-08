**定义**:  
1. 高层模块不应该依赖低层模块,两者都应该依赖其抽象。
2. 抽象不应该依赖细节。
3. 细节应该依赖于抽象。  

每一个逻辑的实现都是由原子逻辑组成的,不可分割的原子逻辑就是低层模块,原子逻辑的再组装就是高层模块。抽象就是指接口或抽象类,不可被实例化的。细节就是实现类
实现接口或者抽象类而产生的类就是细节,其特点就是可直接被实例化。依赖倒置的原则的表现就是：
- 模块间的依赖通过抽象发生，实现类之间不发生直接的依赖关系，其依赖关系是通过接口或者抽象类产生的。
- 接口或者抽象类不依赖于实现类。
- 实现类依赖于接口或者抽象类。 

即面向接口编程。代码实现如下：
```cpp
class iCar{
public:
    virtual void run()=0;
};

class Benz:public iCar{
public:
    void run(){
        printf("the benz car run");
    };
};

class BMW:public iCar{
public:
    void run(){
        printf("the bmw car run");
    };
};

class iDriver{
public:
    virtual void DirveCar(iCar* car)=0;
};

class Driver:public iDriver{
public:
    void DirveCar(iCar* car){
        car->run();
    };
};

int main(int argc, const char * argv[]) {
    // insert code here...
    iDriver* laosiji = new Driver();
    iCar* bmw = new BMW();
    laosiji->DirveCar(bmw);
    return 0;
}
```
依赖倒置原则的本质就是通过抽象(接口或抽象类)使各个类或模块的实现彼此独立,不相互影响,实现模块之间的松耦合。在实际使用中要遵循以下几点:  
1. 每个类尽量都有接口或抽象类
2. 变量的表面类型尽量是接口或抽象类
3. 任何类都不应该从具体类派生
4. 尽量不要重写基类的函数
5. 结合里氏替换原则使用
  
  
