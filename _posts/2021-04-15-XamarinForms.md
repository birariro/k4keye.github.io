---
layout: post
title: XamarinForm
author: k4keye
date: 2021-04-15
categories : Survival
---
<br/>
<br/>

여기에 작성되는 내용은 XamarinForm 을 통해 Android를 개발하면서 <br/>
나중에 필요할 때 다시 찾기도 하고 <br/>
다음 사람이 왔을 때 내가 같이 없는 경우 헤매지 않게 하기 위한 내용이다.<br/>
<br/>
<br/>



## 1. 데이터 관리 
같은 포맷의 데이터를 한곳에서 관리하지 않고 이곳저곳에서 하드코딩하여 사용하다 보면 <br/>
간단하게 변경하려할때도 위치를 찾아야하고 <br/>
큰 변경 있을 때는 매우 힘들어진다.<br/>
그래서 같은 포멧의 데이터는 한곳에서 관리하고 다른곳에서 참조하여 사용하게 하는것이 좋다.<br/><br/>
~[1.1 static readonly로 문자열 관리](https://github.com/k4keye/XamarinDocument/blob/main/1/ReadonlyString.md)~<br/>
[1.2 Resources파일로 문자열 관리](https://github.com/k4keye/XamarinDocument/blob/main/1/Resources.md) <br/>
[1.3 ResourceDictionary로 Color,Style 관리](https://github.com/k4keye/XamarinDocument/blob/main/1/ResourceDictionary.md) <br/>
## 2. MVVM
MVVM으로 개발을 하는 방법을 기록할것이다.<br/><br/>
[2.1 ViewModel](https://github.com/k4keye/XamarinDocument/blob/main/2/VIewModel.md) <br/>
[2.2 ICommand](https://github.com/k4keye/XamarinDocument/blob/main/2/ICommand.md) <br/>
[2.3 ViewBinding](https://github.com/k4keye/XamarinDocument/blob/main/2/VIewBinding.md) <br/>
[2.3 INotifyPropertyChanged](https://github.com/k4keye/XamarinDocument/blob/main/2/INotifyPropertyChanged.md) <br/>

##  3. Null 체크
개발을 하면서 Null로 인한 문제가 많이 발생한다.<br/>
사용자에게 입력받는 Control에서도 발생하고 예상하지 못한 로직에서 Null이 발생한다<br/>
따라서 흐름에있어 중요한 부분은 반드시 Null체크를 해야한다.<br/><br/>
[3.1 ControlNullCheck](https://github.com/k4keye/XamarinDocument/blob/main/3/ControlNullCheck.md)  <br/>

## 4. Control
안드로이드 개발 특성상 로직만을 개발하는것이 아닌 사용자 환경에서의 UI 개발이 필요하다.<br/>
안드로이드 네이티브의 경우 레퍼런스가 많이 존재하지만<br/>
Xamarin의 레퍼런스는 매우 적기때문에 이곳에서 기록할 것이다.<br/><br/>
[4.1 Enrty](https://github.com/k4keye/XamarinDocument/blob/main/4/Entry.md)  <br/>

## Android
XamarinForms는 AOS 와 IOS 를 공유코드로 동시개발하는 장점이있지만<br/>
AOS,IOS 의 공통적인 부분만을 공유코드로 작성할수있다.<br/>
즉 공통된 기능이 아닌경우 각 네이티브 기능을 사용해야 하는경우가 반드시 생기게되는데<br/>
이곳에서는 각 네이티브 자원을 사용하는 방법을 기록할 것이다.<br/><br/>
[안드로이드 자원 사용하기](https://github.com/k4keye/XamarinDocument/blob/main/android/DependencyService.md) <br/>

## Development
XamarinForms 에서 특정 기능을 구현한 예시를 이곳에 기록할것이다.<br/><br/>
[FCM](https://github.com/k4keye/XamarinDocument/blob/main/development/FCM.md) <br/>


## CodeConventions
아직 미숙하지만 코드작성 규칙을 조금씩 정해갈 예정이다.<br/><br/>
[변수](https://github.com/k4keye/XamarinDocument/blob/main/codeConventions/%EB%B3%80%EC%88%98.md) <br/>
[선언](https://github.com/k4keye/XamarinDocument/blob/main/codeConventions/%EC%84%A0%EC%96%B8.md) <br/>
[함수](https://github.com/k4keye/XamarinDocument/blob/main/codeConventions/%ED%95%A8%EC%88%98.md) <br/>
[주석](https://github.com/k4keye/XamarinDocument/blob/main/codeConventions/%EC%A3%BC%EC%84%9D.md) <br/>
[AXML](https://github.com/k4keye/XamarinDocument/blob/main/codeConventions/XAML.md) <br/>
[RecycleViewModel](https://github.com/k4keye/XamarinDocument/blob/main/codeConventions/RecycleViewModel.md) <br/>



## ETC
XamarinForms 개발을 진행하면서 발생되었던 문제 해결 방법이나 기타 사용방법을 기록할것이다.<br/><br/>
[EUC-KR 인코딩 문제](https://github.com/k4keye/XamarinDocument/blob/main/etc/euc-kr.md) <br/>
[StackTrace](https://github.com/k4keye/XamarinDocument/blob/main/etc/StackTrace.md)<br/>
[SyncFusionListviewHeader](https://github.com/k4keye/XamarinDocument/blob/main/etc/SyncFusionListViewHeader.md) <br/>
[동기에서 비동기호출](https://github.com/k4keye/XamarinDocument/blob/main/etc/%EB%8F%99%EA%B8%B0%EC%97%90%EC%84%9C_%EB%B9%84%EB%8F%99%EA%B8%B0%ED%98%B8%EC%B6%9C.md) <br/>
[CodeSnippet](https://github.com/k4keye/XamarinDocument/blob/main/etc/CodeSnippet.md)<br/>

___

### :fire: Writer
[k4keye](https://github.com/k4keye) <br/>
[JJinggg](https://github.com/JJinggg)
