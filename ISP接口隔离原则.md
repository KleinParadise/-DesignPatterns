**定义**:  接口尽量细化,建立单一的接口,接口中尽量容纳少一些可变的因素即接口提供的方法尽量少。  
如现在有两种类型的美女,一种是气质型美女,一种是面容较好型美女。因为都是美女,将其封装到一个接口中,代码如下：
```cpp
class iPettyGirl{
public:
    virtual void GookLooking()=0;//面容较好
    virtual void NiceFigure()=0;//身材好
    virtual void GreatTemperament()=0;//气质好
};

class PettyGirl:public iPettyGirl{
public:
    void GookLooking(){
        printf("GookLooking");
    };//面容较好
    void NiceFigure(){
        printf("NiceFigure");
    };//身材好
    void GreatTemperament(){
        printf("GreatTemperament");
    };//气质好
};

//星探抽象类
class AbstractSearcher{
public:
    AbstractSearcher(iPettyGirl* girl):pGirl(girl){};
    virtual void ShowGirlInfo()=0;//显示美女信息
protected:
    iPettyGirl* pGirl;
};

class Searcher:public AbstractSearcher{
public:
    Searcher(iPettyGirl* girl):AbstractSearcher(girl){
        
    }
    //显示美女信息
    void ShowGirlInfo(){
        pGirl->GookLooking();
        pGirl->GreatTemperament();
        pGirl->NiceFigure();
    };
};

int main(int argc, const char * argv[]) {
    // insert code here...
    iPettyGirl* jing = new PettyGirl();
    AbstractSearcher* searcher = new Searcher(jing);
    searcher->ShowGirlInfo();
    return 0;
}
```
将PettyGir的行为封装到一个接口,导致Searcher无法对PettyGir进行类型区分。按照接口隔离原则,应该针对PettyGir的类型来设计接口,代码如下：
```cpp
class iGreatTemperamentGirl{
public:
    virtual void GreatTemperament()=0;//气质好
};

class iGoodBodyGirl{
public:
    virtual void GookLooking()=0;//面容较好
    virtual void NiceFigure()=0;//身材好
};

//星探抽象类
class AbstractSearcher{
public:
    AbstractSearcher(iGoodBodyGirl* girl):pGoodBodyGirl(girl){};
    AbstractSearcher(iGreatTemperamentGirl* girl):pGreatTemperamentGirl(girl){};
    virtual void ShowGirlInfo()=0;//显示美女信息
protected:
    iGoodBodyGirl* pGoodBodyGirl;
    iGreatTemperamentGirl* pGreatTemperamentGirl;
};
```
通过这样的重构,不管需要那种类型的美女,都可以保证接口的稳定。以上把臃肿的接口转化为两个独立的接口所依赖的原则就是接口隔离的原则。AbstractSearcher依赖两
个专用的接口比依赖一个综合的接口要灵活。
接口隔离原则是对接口进行规范,包含以下4个条件:
1. 接口尽量要小
2. 接口要高内聚即提高接口,类,模块的处理能力,减少对外的交互。
3. 定制服务即只提供访问者需要的方法
4. 接口设计是有限度的,也不能无限小或者无限大。要根据实际情况来判断。
