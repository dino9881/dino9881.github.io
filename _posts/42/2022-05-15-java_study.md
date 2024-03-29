---
layout: post
title: 2022-04-22-TIL
subtitle: "Java 시작하기"
categories: 42seoul
tags:
- 42
- TIL
- Java
comments: true
published: false
---

# Java 시작하기

## Java란?
Java는 썬 마이크로시스템즈에서 1995년에 개발한 객체 지향 프로그래밍 언어로, 제임스 고슬링이 창시했다. 2010년 오라클이 인수하면서 Java의 저작권을 소유하였다. 

## Java의 특징
소스 코드를 기계어로 직접 컴파일하여 링크하는 C/C++의 컴파일러와는 달리 자바 컴파일러는 바이트 코드인 클래스 파일(.class)을 생성하고 JVM(자바 가상 머신)에서 실행하여 플랫폼에 독립적인 언어라는 특징이 있다. 

## Keyword 단위로 공부하기  

1. #개발환경 구축  
-  JDK(Java Development Kit)을 설치하면 javac 라는 컴파일러가 제공되지만 통합 개발 환경은 제공해 주지 않기 때문에, 반드시 개발용 프로그램을 써야 한다. 대표적으로 이클립스, ItelliJ 등이 있다. Jetbrain에서는 학생 이메일을 인증하면 IntelliJ의 상위 버전도 무료로 지원해주기 때문에 42서울 이메일을 이용하여 사용하기로 결정했다.  
2. #클래스
- 클래스는 객체지향프로그래밍을 위한 객체들을 만드는 틀 이다. 클래스는 변수 , 메소드를 가지며 만들어진 객체들에 동일한 속성들을 부여한다. 
3. #패키지
- 패키지는 비슷한 성격의 자바 클래스들을 모아 놓은 디렉토리 이다. 클래스 안에서 다른 클래스를 사용하려면 import를 해야한다. 같은 패키지 안에서는 import를 사용하지 않아도 되지만, 클래스명이 동일한 경우 충돌이 발생 할 수 있기때문에 자바 클래스는 반드시 패키지명을 포함하는 것이 올바른 방법이다. 
4. #자바 실행하기 
5. #.class 파일위치
- 빌드를 하게 되면 /out/production/"프로젝트 이름"/ 경로에 .class 파일들이 생기게 된다.
6. #자바 코딩 컨벤션
- 코딩 컨벤션 가독성을 높이고 관리하기 쉬운 코드를 작성하기 위한 일종의 코딩 스타일 규약이다.

### Java coding convention
#### 파일 이름
소스 코드 -> .java
자바 바이트코드 -> .class 
#### 소스 파일의 구조
- 라이센스 또는 저작권 정보
- package 
- import
- 최상위 클래스
- 각 섹션들은 하나의 빈 줄로 구분한다. 

### 참조변수
#### 변수 Type
- 기본Type: byte, short, char, int, long, float, double, boolean 8개 타입이 있고 , 변수에는 값 자체가 저장된다. 
- 참조Type: 기본타입을 제외하고 배열, 열거, 클래스, 인터페이스 등을 말한다. 변수에는 객체 의 메모리가 저장된다.  


### Java 메모리 구조
#### 메소드 영역 
메소드 영역은 JVM이 시작할 때 생성되고 모든 쓰레드가 공유하는 영역이다. 클래스들을 읽어 클래스 별로 
static field 와 constant, method code, constructor code 들을 분류해서 저장한다. 

#### 힙 영역
힙 영역은 객체와 배열이 생성되는 영역이다. 생성된 객체와 배열들은 다른 객체의 필드에서 참조 될 수 있다. 만일 참조하는 변수나 필드가 없다면 의미없는 객체이므로 garbage collector로 처리한다. 

#### 스택영역
JVM 스택은 메소드를 호출할때마다 프레임을 추가하고 메소드가 종료되면 해당 프레임을 제거하는 동작을 수행한다. 


### this
1. 객체 자신의 속성을 나타내는 this -> 생성자에서  this.name = name 처럼 사용할때 자기 자신의 속성을 나타내도록 사용할 수 있다.  
2. 클래스에 오버로딩된 다른 생성자를 호출 할때 사용 -> 하나의 클래스에 여러개의 생성자가 오버로딩 되어 있을 때 일부분을 제외하고는 서로 중복된 코드를 가지고 있는 경우가 많이 있는데 이때 내부에 정의된 다른 생성자를 호출하여 코드의 중복을 피하고 깔끔한 소스를 작성 할 수 있다.  
3. 메소드에서 자기 자신의 참조값 을 리턴하고 싶은 경우에 return this; 형식으로 사용한다.  

