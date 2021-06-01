---
layout: post
title: ViewBinding으로 FindViewByid 랑 작별하기
author: k4keye
date: 2021-06-01
categories : Android
---

## 1 서론
___
## **1.1 FindViewByid**
<br/>
안드로이드에서 Layout에 존재하는 컴포넌트를 객체로 사용하려면<br/>
FindViewByid를 통하여 작업을 했어야 했다.<br/>
<br/>
만약 접근하려는 컴포넌트가 5개, 10개만 넘어도<br/>
class 파일에는 FindViewByid 가 많이 붙게 되는 걸 볼 수 있다.<br/>
또한 개발자의 실수로 인해 잘못된 컴포넌트를 참조하려 하면 null이 발생하는 문제가 있다.<br/>
<br/>
예전에 Kotlin에서는 kotilin-android-extensions 을 통해 FindViewByid 없이도 컴포넌트에 접근하여 사용할 수 있는 게 큰 매력으로 다가왔지만 <br/>
이제는 여러 문제점으로 인해 kotilin-android-extensions를 기본으로 제공하지 않는다.<br/>
따라서 이제는 ViewBinding를 사용하도록 권장하고 있다.<br/><br/>


## **1.2 ViewBinding**
<br/>
build.gradle에 viewBinding 속성을 활성화시키는 것으로 사용할 수 있으며<br/>
해당 모듈에 있는 layout 파일에 대한 binding class 가 자동으로 생성된다.<br/>
<br/>
이렇게 생성된  binding class를 통해 해당 layout에 있는 모든 컴포넌트에 참조가 가능해진다.<br/>
즉 FindViewByid를 대체할 수 있게 된다.<br/><br/>

## 2 ViewBinding 사용하기
___
## **2.1 ViewBinding 활성화**
<br/>

```gradle
android {
    buildFeatures {
        viewBinding true
    }
}
```
이렇게 설정하는 것으로 Layout 파일에 대한 binding class 파일이 자동으로 생성되며<br/>
카멜 표기법으로 변환되고 이름 끝에 binding를 붙인 이름으로 생성된다.<br/>
<br/>
activity_main.xml -> ActivityMainBinding로 생성된다.<br/><br/>


## **2.2 Layout에 컴포넌트 배치**
<br/>

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">

    <TextView
        android:id="@+id/tv"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        android:textSize="15sp"/>
    <Button
        android:id="@+id/btn"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="버튼1"
        />

</LinearLayout>
```
간단하게 TextView, Button 을 하나씩 배치하였다.<br/><br/>


## **2.3 Activity Class 파일 수정**
<br/>

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        
    }
}
```
기본적으로 만들어진 Class는 위와 같은 모양을 하고 있다.<br/>
이제 이곳에 ViewBinding를 연결하면 된다.<br/>

```kotlin
class MainActivity : AppCompatActivity() {

    private lateinit var binding : ActivityMainBinding
	
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
	}
}
```
기존에 setContentView로 연결되어 있는 Layout이  아닌<br/>
Binding Class의 인스턴스를 얻기 위해 inflate()를 사용하여 <br/>
rootView를 매개변수로 넣어주는 것으로 View를 참조할 수 있게 된다.<br/>


## **2.4 컴포넌트 접근**
<br/>

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var binding : ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)


        binding.btn.setOnClickListener{
            binding.tv.text="클릭";
        }
	}
}
```
<br/><br/>

[ViewBinding Android Developers ](https://developer.android.com/topic/libraries/view-binding)
