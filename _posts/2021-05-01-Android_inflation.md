---
layout: post
title: Android inflation 란
author: k4keye
date: 2021-05-01
categories : Android
---

## 1 Inflation
___
### **1.1 서론**
<br/>
Android 에서의 inflation 을 알아보려한다.<br/>
<br/>
inflation은 xml 레이아웃 파일로 정의한 정보를 런타임 중에 메모리 상에 객체로 만들어주어 화면에 보여주는 과정을 의미한다.<br/>
즉 Android 개발을 하면서 View를 만들기 위해 작업하였던 xml을 실제로 사용할 수 있게 해주는 작업이라고 할 수 있다.<br/>

___
### **1.2 Inflation 사이클**

<p align="center">
    <img src="https://github.com/k4keye/k4keye.github.io/blob/master/images/android/inflation.png?raw=true"/>
</p>

이미지를 보고 간단하게 확인하면 xml 을 정의하고 실행 시 메모리로 xml을 로딩하여 화면에 보여주는 순서이다.<br/>
이 일련의 작업은 xml 레이아웃 파일을 실제로 사용할 수 있도 록에서 view ID 를 설정하고 해당 ID가 R 파일에 주소값으로 적용되어<br/>
findViewById 메서드 와 Id를 활용하여 코드상으로 View 객체를 가져와 제어할 수 있게 하는 것이다.<br/>

### **1.3 Inflation 종류**

Inflation에는 두 가지 방식이 있다.

#### **1.3.1 전체화면**
흔히 xml과 매핑되는 Class 파일에는 자동적으로 setContentView 가 적용되어 있는 모습을 볼 수 있다.<br/>
이것이 Inflation 이다.<br/>
setContentView 는 전체 화면에 뷰를 지정할 때 사용된다.<br/>

#### **1.3.2 일부화면**

LayoutInflater 를 사용하여 작업하는 경우 전체 화면이 아닌 일부를 차지하는 요소들을 화면에 보여줄 때 사용된다.<br/>
물론 이 또한 Inflation 이다.<br/>

### **1.4 결론**
결국 View를 그리기 위해 작성한 xml은 Inflation을 통해 제어를 할 수 있던 것이었다.<br/>
그래서 Inflation 하기 전에 해당 View에 리소스에 접근하게 되면 NULL이 발생하게 되는 것을 볼 수 있다.<br/>
즉 Class 파일에서 View 리소스를 당연하게 사용하고 있던 것은 Inflation 작업을 하였기 때문에 가능하던 것이다.<br/>
