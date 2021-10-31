# MVC, MVP, MVVM

- **패턴이 존재하는 이유** : 협업, 유지보수, 테스트의 용이성
- **프레임 워크 패턴들의 공통적인 특징** : 화면에 보여주는 로직과 실제 데이터가 처리되는 로직을 분리한다.

<br/>

## MVC 패턴

> Model + View + Controller

![](https://i.imgur.com/klJFmk0.png)

<br/>

### 구조

- **Model** : 프로그램에서 사용되는 데이터와 데이터를 처리하는 부분
- **View** : 사용자에게 제공되어 보여지는 UI 부분
- **Controller** : 사용자의 입력(Action)을 받고 처리하는 부분

<br/>

### 동작
1. 사용자의 Action이 Controller에 들어온다.
2. Controller는 사용자의 Action을 확인하고 Model을 업데이트하고 불러온다.
3. Controller는 Model을 나타내줄 View를 선택한다.
4. View는 Model을 이용해 업데이트된 후 화면을 나타낸다.

<br/>

### 특징
- View-Controller 관계: One-to-Many(일대다 관계). Controller 는 view를 선택할 수 있기에 view를 여러개 관리할 수 있다.
- View는 Controller의 존재를 모른다.
- View 는 Model 의 변화에 대해 직접적으로 알지 못한다. 또한 Controller 는 view를 선택하지만 view를 직접 업데이트하지 않는다.

```
참고 - MVC에서 View가 업데이트 되는 방법
- View가 Model을 이용하여 직접 업데이트 하는 방법
- Model에서 View에게 Notify 하여 업데이트 하는 방법
- View가 Polling으로 주기적으로 Model의 변경을 감지하여 업데이트 하는 방법.
```

<br/>

### 장점
- 가장 단순하다.
- 보편적으로 사용된다.

<br/>

### 단점
- View와 Model이 서로 의존적이다.
    → 어플리케이션이 커지고 복잡해질 수록 유지보수가 어려워진다.
    → 보완 : MVP 패턴

<br/>

## MVP 패턴

> Model + View + Presenter

![](https://i.imgur.com/whnnIXt.png)

<br/>

### 구조
- **Presenter** : View에서 요청한 정보를 Model로부터 가공해서 View로 전달하는 부분

<br/>

### 동작
1. 사용자의 Action이 View에 들어온다.
2. View는 데이터를 Presenter에 요청한다. 
3. Presenter는 Model에 데이터를 요청한다.
4. Model은 Presenter에서 요청받은 데이터를 응답한다.
5. Presenter는 View에서 데이터를 응답한다.
6. View는 Presenter가 응답한 데이터를 이용하여 화면을 나타낸다.

<br/>

### 특징
- Model과 View는 MVC와 동일하지만 사용자 입력을 View에서 받는다.
- 그리고 Model과 View는 각각 Presenter와 상호작용
- Presenter는 View와 Model의 인스턴스를 가지고 있어 둘을 연결하는 접착제 역할을 한다.
- Presenter와 View는 1:1 관계이다.
- View와 Model의 의존성이 없는 대신 View와 Presenter가 강한 의존성을 가진다.
    → 보완 : MVVM 패턴

<br/>

## MVVM 패턴

> Model + View + ViewModel

![](https://i.imgur.com/vNoa6oO.png)

<br/>

### 구조
- Presenter 대신 ViewModel
- **ViewModel** : View를 표현하기 위해 만들어진 View를 위한 Model, View를 나타내 주기 위한 Model이자 View를 나타내기 위한 데이터 처리를 하는 부분이다.

<br/>

### 동작
1. 사용자의 Action이 View에 들어온다.
2. View에 Action이 들어오면, Command 패턴으로 ViewModel에 Action을 전달한다.

> 커맨드 패턴(Command pattern)이란 요청을 객체의 형태로 캡슐화하여 사용자가 보낸 요청을 나중에 이용할 수 있도록 매서드 이름, 매개변수 등 요청에 필요한 정보를 저장 또는 로깅, 취소할 수 있게 하는 패턴이다.

3. ViewModel은 Model에게 데이터를 요청한다.
4. Model은 ViewModel에게 요청받은 데이터를 응답한다.
5. ViewModel은 응답받은 데이터를 가공하여 저장한다.
6. View는 ViewModel과 Data Binding하여 화면을 나타낸다.

> 데이터 바인딩은 두 데이터 혹은 정보의 소스를 모두 일치시키는 기법이다. 즉 화면에 보이는 데이터와 브라우저 메모리에 있는 데이터를 일치시킨다.

<br/>

### 특징
- Command 패턴과 Data Binding을 이용해 View와 ViewModel 사이의 의존성을 없앴다.
- ViewModel과 View는 1:N 관계이다.


<br/>

### 장점
- View와 Model 사이의 의존성, View와 ViewModel 사이의 의존성이 없다.
- 각 부분이 독립적이기 때문에 모듈화하여 개발할 수 있다.

<br/>

### 단점
- ViewModel의 설계가 어렵다.
