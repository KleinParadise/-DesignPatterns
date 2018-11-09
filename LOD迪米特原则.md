**定义**:一个类应该对自己需要耦合或者调用的类知道得最少。
1. 一个类应该之和自己有直接关系的类交流。  
  如何判断一个类与另一个类是否有直接关系？  
  出现在该类成员变量,函数的输入输出参数中的类称为与该类有直接关系,出现在该类函数体内部的类称为与该类无直接的关系。
```cpp
#include <iostream>
#include <list>
using namespace std;

class Girl{
    
};

class GroupLeader{
public:
    void CountGirls(list<Girl>& girlList){
        cout<<"the girls count ="<<girlList.size()<<endl;
    };
};

class Teacher{
public:
    void commond(GroupLeader* pLeader){
        list<Girl> girlList(20);
        pLeader->CountGirls(girlList);
    };
};

int main(int argc, const char * argv[]) {
    // insert code here...
    Teacher* pT = new Teacher();
    GroupLeader* pG = new GroupLeader();
    pT->commond(pG);
    return 0;
}
```
Girl类出现在Teacher类的commond函数体内,因此与Teacher类无直接的关系。按照一个类应该之和自己有直接关系的类交流原则,Teacher类不应该与自己没直接关系的Girl
类出现交流。函数是类的一个行为,类不知道自己的行为与其他类产生了依赖关系，违背了迪米特原则。按照原则应做出如下修改:
```cpp
#include <iostream>
#include <list>
using namespace std;

class Girl{
    
};

class GroupLeader{
public:
    GroupLeader(list<Girl>& girl):pGirlList(girl){};
    void CountGirls(){
        cout<<"the girls count ="<<pGirlList.size()<<endl;
    };
private:
    list<Girl>& pGirlList;
};

class Teacher{
public:
    void commond(GroupLeader* pLeader){
        pLeader->CountGirls();
    };
};

int main(int argc, const char * argv[]) {
    // insert code here...
    Teacher* pT = new Teacher();
    list<Girl> girlList(20);
    GroupLeader* pG = new GroupLeader(girlList);
    pT->commond(pG);
    return 0;
}
```
2. 一个类与自己有直接的关系的类之间也是有距离的。两者之间耦合性要做到最低。
```cpp
class Wizard{
public:
    int Firs(){
        return rand();
    };
    
    int Second(){
        return rand();
    };
    
    int third(){
        return rand();
    };
};

class InstallSoftware{
public:
    void InstallWizard(Wizard* pw){
        if(pw->Firs() > 50){
            if(pw->Second() > 40){
                if(pw->third()){
                    pw->Firs();
                }
            }
        }
    };
};

int main(int argc, const char * argv[]) {
    // insert code here...
    Wizard* pw = new Wizard();
    InstallSoftware* pi = new InstallSoftware();
    pi->InstallWizard(pw);
    return 0;
}
```
上述代码Wizard类将自己的太多函数暴露给InstallSoftware类,耦合关系变的异常牢固。违背了第二条原则。应该将Wizard类的函数尽量少的暴露给InstallSoftware类，
修改如下:  
```cpp
class Wizard{
  private: 
    int Firs(){
        return rand();
    };
    
    int Second(){
        return rand();
    };
    
    int third(){
        return rand();
    };
 public:   
    void install(){
        if(this->Firs() > 50){
            if(this->Second() > 40){
                if(this->third()){
                    this->Firs();
                }
            }
        }
    };
};

class InstallSoftware{
public:
    void InstallWizard(Wizard* pw){
        pw->install();
    };
};

int main(int argc, const char * argv[]) {
    // insert code here...
    Wizard* pw = new Wizard();
    InstallSoftware* pi = new InstallSoftware();
    pi->InstallWizard(pw);
    return 0;
}
```
一个类公开的public属性或函数越多,涉及修改的面也就越大,产生的风险也就越多。为了降低类与直接类之间的耦合,要尽量减少public的属性或方法。

3. 如果一个函数放在本类中,既不增加类间关系,也不对本类产生负面影响,那就放置在本类中。
