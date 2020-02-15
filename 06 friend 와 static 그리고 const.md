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



 
### 1.2.1. 내용1
```
내용1
```

***
# 2. 대주제
> 인용
## 2.1. 소 주제
### 2.1.1. 내용1
```
내용1
```   

***
# 3. 대주제
> 인용
## 3.1. 소 주제
### 3.1.1. 내용1
```
내용1
```
