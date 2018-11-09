**定义**: 一个软件实体应该通过扩展来实现变化,而不是通过修改已有代码来实现变化。 

什么是软件实体？  
    软件实体包括项目或者软件产品中按照一定的逻辑规则划分的模块,抽象和类,函数三部分。
    
通过书店销售书籍为例，来理解开闭原则。代码如下:  
```cpp
#include <iostream>
#include <vector>
using namespace std;

//书籍抽象类
class IBook{
public:
    virtual string getName()=0;
    virtual int getPrice()=0;
    virtual string getAuthor()=0;
};
//小说类
class NoveBook : public IBook{
public:
    NoveBook(){};
    NoveBook(string n,int p,string a):name(n),price(p),author(a){
        
    }
    string getName(){
        return name;
    };
    int getPrice(){
        return price;
    };
    string getAuthor(){
        return author;
    };
private:
    string name;
    int price;
    string author;
};


int main(int argc, const char * argv[]) {
    // insert code here...
    vector<IBook*> vecBook;
    NoveBook* book1 = new NoveBook("天龙八部",5,"金庸");
    NoveBook* book2 = new NoveBook("鹿鼎记",3,"金庸");
    NoveBook* book3 = new NoveBook("射雕英雄传",2,"金庸");
    vecBook.push_back(book1);
    vecBook.push_back(book2);
    vecBook.push_back(book3);
    
    for(int i = 0;i<vecBook.size();++i){
        IBook* p = vecBook[i];
        cout<<"bookName="<<p->getName()<<" thePice ="<<p->getPrice()<<" the author ="<<p->getAuthor()<<endl;
    }
    
    return 0;
}
``` 

这时有一个新需求,所有价格两元以上的书要8折出售?此时有三种可修改方案:  
1. 修改接口。IBook类中新增getOffPrice()函数。这样修改的后果是实现类NoveBook要对应修改。mian函数也要对应修改,并且违背了接口应该稳定可靠不应经常修改的
原则。因此该方案不是最佳方案。  
2. 修改实现类。在实现类NoveBook中对getPrice()函数进行直接处理。但是这样没办法获取到书籍的原价格，所以也不是最佳方案。  
3. 通过扩展实现变化。增加一个OffNoveBook的子类,继承NoveBook，重写getPrice()函数。这样我们只需修改main函数对应的书籍类型就能完成此需求,并且改动最小。
此方案为最佳方案。实现代码如下:
```cpp
#include <iostream>
#include <vector>
using namespace std;

//书籍抽象类
class IBook{
public:
    virtual string getName()=0;
    virtual int getPrice()=0;
    virtual string getAuthor()=0;
};
//小说类
class NoveBook : public IBook{
public:
    NoveBook(){};
    NoveBook(string n,int p,string a):name(n),price(p),author(a){
        
    }
    string getName(){
        return name;
    };
    int getPrice(){
        return price;
    };
    string getAuthor(){
        return author;
    };
protected:
    string name;
    int price;
    string author;
};

//打折小说类
class OffNoveBook : public NoveBook{
public:
    OffNoveBook(string n,int p,string a):NoveBook(n,p,a){}

    int getPrice(){
        return price * 0.8;
    };
};

int main(int argc, const char * argv[]) {
    // insert code here...
    vector<IBook*> vecBook;
    NoveBook* book1 = new OffNoveBook("天龙八部",5,"金庸");
    NoveBook* book2 = new OffNoveBook("鹿鼎记",3,"金庸");
    NoveBook* book3 = new OffNoveBook("射雕英雄传",2,"金庸");
    vecBook.push_back(book1);
    vecBook.push_back(book2);
    vecBook.push_back(book3);
    
    for(int i = 0;i<vecBook.size();++i){
        IBook* p = vecBook[i];
        cout<<"bookName="<<p->getName()<<" thePice ="<<p->getPrice()<<" the author ="<<p->getAuthor()<<endl;
    }
    return 0;
}
```

为什么要使用开闭原则？  
1. 开闭原则对测试的影响。通过扩展来实现业务逻辑的变化，可以只针对扩展进行单元测试,减少工作量。
2. 开闭原则可以提高复用性
3. 开闭原则可以提高可维护性
4. 面向对象开发的要求

如何使用开闭原则?  
1. 抽象约束。  
    1. 通过接口或抽象类约束扩展,对扩展进行边界限定,不允许出现在接口或抽象类中不存在的public函数。
    2. 参数类型,引用对象尽量使用接口或抽象类,而不是实现类。
    3. 抽象层尽量保持稳定,一单确定了减少修改。
2. 通过元数据来控制模块行为。即通过配置数据来实现函数功能。

  
