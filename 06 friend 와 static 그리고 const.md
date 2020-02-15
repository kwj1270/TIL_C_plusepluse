06 friend 와 static 그리고 const
=======================
# 1. const와 관련해서 아직 못다한 이야기 
## 1.1. const 객체와 const 객체의 특성들
```
const int num = 10;
const SoSimple sim(20);
```
const 선언은 변수는 물론 객체에도 선언할 수 있다.     
객체에 const 선언을 하는 것은 **객체의 대이터 변경을 허용하지 않겠다!**라는 의미이다.     
   
그리고 이렇게 const 선언을 한 객체를 대상으로는 **const로 선언되지 않는 멤버함수의 호출이 불가능하다.**   

```
int main(void)
{
  const SoSimple obj(7);
  // obj.AddNum(20);
  obj.ShowData();
  return 0;
}
```
```
class SoSimple
{
  priavte:
    int num;
  public:
    SoSimple(int n) : num(n)
    {}
    
    SoSimple& AddNum(int n)
    {
      num += n;
      return *this;
    }
    void ShowData() const
    {
      cout<<"num: "<<num<<endl;
    }
};
```
객체를 생성할 때 바로 초기화 해주는 생성자는 사용이 가능하지만      
이렇게 한번 값이 할당 되면 const 객체는 const 선언된 메소드만 호출이 가능하다 (값 변동 유무 상관없이)         
     
## 1.2. const와 함수 오버로딩  
C++의 함수 오버로딩 조건에는 한가지 조건이 더 있는데 그것이 const 유무이다.   
(매개변수 타입, 매개변수 개수, const 유무)   
```
void SimpleFunc()
{
  cout<<"SimpleFunc: " <<num<<endl;
}
void SimpleFunc() const
{
  cout<<"const SimpleFunc: " <<num<<endl;
}
```
하지만 const 유무를 가지고서는 어떠한 메소드를 호출할지에 대해서는 의문점이 든다.       
C++에서는 const 객체는 const 메소드만 호출 가능하기에 const 메소드를 사용하게 한다.      
그리고 const 선언이 되어있지 않은 객체는 일반 메소드를 호출하게끔 규정되었다.       
     
**주의 점**      
```
void YourFunc(const SoSimple &obj){
  obj.SimpleFunc();
} 
```   
const 참조자라고 해서 const 객체만 받을 수 있는 것은 아니다.  
기존에 const 참조자는 인자로 리터럴 및 변수(또는 객체)를 받을 수 있었던 것처럼  
const 객체도 물론 받을 수 있다고 이해를 하는 것이 더 편하다.   
위 코드에서는 받는 객체에 따라 실행 함수를 달리 하고 있다.     
   
***
# 2. 클래스와 함수에 대한 friend 선언     
A클래스가 B클래스 , C클래스와 전역 메소드 friend 선언을 했다고 가정을 하면  
B클래스, C클래스, 전역 메소드는 A클래스의 priavte 멤버에 접근이 가능해진다.  
즉, friend 선언은 나의 private에 접근을 허용하겠다는 의미이다.  

다시 예를 들자면 A클래스가 B클래스를 friend로 선언을 했다면     
B클래스의 멤버 함수들은 A클래스의 private 에 접근할 수 있다.     
하지만 그 반대로 A클래스는 B클래스의 private에 접근할 수는 없다.     
      
만약 B클래스도 A클래스를 friend로 지정을 할 경우     
양측의 private에 접근이 가능한 형태가 되지만 이는 정보은닉에 위배되는 형태가 될 것이다.   
그래서 특정한 경우 일 때 사용하는 것이 좋다. (가급적 사용하지 않도록 하자)   
          
## 2.1. 클래스에 fiend 선언하기
   
**Boy.class**
```
class Boy
{
private:
   int height;
   friend class Girl;
public:
   Boy(int len) : height(len)
   { }
   . . . . .
};
```
friend 선언은 private 멤버의 접근을 허용하는 선언이다.    
friend 선언은 정보 은닉에 반하는 선언이기 때문에 매우 제한적으로 선언되어야 한다.    
       
프랜드 선언은 값을 할당해주지 않아도 된다.(생성자에서 제외된 것을 보면 알 수 있다.)     
   
**Girl.class**
```
class Girl
{
private: 
   char phNum[20];
public: 
   Girl(char * num)
   {
      strcpy(phNum, num);
   }
   void ShowYourFriendInfo(Boy &frn)
   {
      cout<<"His height:"<<frn.height<<endl;
   }
}
```
Girl이 Boy의 friend로 선언되었으므로, private 멤버에 직접 접근이 가능하다.  
     
## 2.2. 함수에 fiend 선언하기

**Point.class**
```
class Point
{
private:
   int x;
   int y;
public:
   Point(const int &xpos, const int &ypos) : x(xpos), y(ypos)
   { }
   friend Point PointOP::PonitAdd(const Point&, const Point&); //클래스의 특정 함수를 대상으로도 friend 가능
   friend Point PointOP::PonitSub(const Point&, const Point&); //클래스의 특정 함수를 대상으로도 friend 가능
   friend void ShowPointPos(const Point&); // 전역변수
};
```
참고로 friend 선언시에 매개 변수명은 넣어주지 않아도 된다.      
   
**PointOP.cpp**
```
Point PointOP::PonitAdd(const Point& pnt1, const Point& pnt2)
{
   opcnt++;
   return Point(pnt1.x+pnt2.x, pnt1.y+pnt2.y);
}
Point PointOP::PonitSub(const Point& pnt1, const Point& pnt2)
{
   opcnt++;
   return Point(pnt1.x-pnt2.x, pnt1.y-pnt2.y);
}
```
**전역**
```
void ShowPointPos(const Point& pos)
{
   cout<<"x :"<<pos.x<<", ";
   cout<<"y :"<<pos.y<<endl;
}
```
    
***    
# 3. C++ 에서의 static  
C언어에서의 static은 전역변수와 지역변수를 대상으로 선언할 수 있었다.  
변수에 static 선언을 할 경우 시작과 동시에 메모리에 올라가서 종료될 때 내려온다.    
  
지역변수에 static은 실질적으로 전역변수와 같이 사용이 가능했고       
전역변수에서의 static은 범위를 파일내로 한정 짓기에 extern 선언을 하더라도 접근이 불가능 해졌다.     

C++은 클래스라는 개념이 추가되었기에 멤버변수와 멤버함수에도 static을 사용할 수 있다.     
클래스의 static 변수도 이와 마찬가지이다.          
클래스를 기반으로 객체가 생성되면 메모리에 올라가서 종료될 때 내려온다.        
**그리고 같은 클래스를 기반으로 하는 모든 객체에서 공통으로 사용할 수 있다.**        
**그리고 이렇게 공통으로 사용하기 위해서는 변수나 함수를 public 선언을 해주는 것이 좋다.**       
(외부에서 ```A::변수``` ```A::함수``` 이렇게 사용도 할 수 있기 때문이다.)         

## 3.1. C언어에서의 이야기한 static
* 전역변수에 선언된 static : 선언된 파일 내에서만 참조를 허용
* 함수내 변수에 선언된 static : 한번만 초기화되고, 지역변수와 달리 빠져나가도 소멸되지 않는다.  

```
void Counter()
{
   static int cnt;
   cnt++;
   cout<<"Current cnt: "<<cnt<<endl;
}
```
```
int main(void)
{
   for(int i=0;i<10;i++){
      Counter();
   }
   return 0;
}
```
```
1 2 3 4 5 6 7 8 9 10
```
   
## 3.2. static 멤버변수(클래스 변수)
```
class SoSimple
{
private:
   static int simObjCnt;   

public:
   SoSimple()
   {
      simObjCnt++;
      cout<<simObjCnt<<"번째 SoSimple 객체"<<endl;
   }
};
int SoSimple::simObjCnt=0; 
```
static 변수는 실질적으로 멤버는 아니기에 밖에서 초기화를 진행해주어야 한다.     
만약 생성자내에서 초기화를 진행해주면 객체 생성할 때마다 초기화가 진행되어서 생성자에 넣으면 안된다.   
또한 클래스에서 ```자료형 변수=값``` 초기화를 진행할 수 없기에 이렇게 작성해야한다.    

static 변수는 객체 별로 존재하는 변수가 아닌, 프로그램 전체 영역에서 하나만 존재하는 변수이다.     
프로그램 실행과 동시에 초기화되어 메모리 공간에 할당된다.      

```
int main(void)
{
   SoSimple sim1; // simObjCnt -> 1 
   SoSimple sim2; // simObjCnt -> 2
   SoSimple sim3; // simObjCnt -> 3
}
```
     
## 3.3. static 멤버변수의 접근 방법
```
class SoSimple
{
public:
   static int simObjCnt;   

public:
   SoSimple()
   {
      simObjCnt++;
   }
};
int SoSimple::simObjCnt=0; 
```
staitic 변수를 public으로 선언을하게 된다면 ```SoSimple::simObjCnt```로 접근이 가능하다.   
```
int main(void)
{
   cout<<SoSimple::simObjCnt<<"번째 SoSimple객체"<<endl;
   SoSimple sim1;
   SoSimple sim2;
   cout<<SoSimple::simObjCnt<<"번째 SoSimple 객체<<endl;"
   cout<<sim1.simObjCnt<<"번째 SoSimple 객체"<<endl;
   cout<<sim2.simObjCnt<<"번째 SoSimple 객체"<<endl; 
}
```
```
0번째 SoSimple 객체
2번째 SoSimple 객체
2번째 SoSimple 객체
2번째 SoSimple 객체
```
