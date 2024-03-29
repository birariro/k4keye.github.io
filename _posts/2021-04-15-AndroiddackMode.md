---
layout: post
title: Android 다크모드 적용하기
author: k4keye
date: 2021-04-15
categories : Android
---
## 1 서론
___
Android 10부터 Dark theme를 지원하기 시작하여<br/>
이제 앱은 기본적으로 밝은 화면, 어두운 화면을 둘 다 제공해야 한다.<br/><br/>

해당 글에서는 기본적으로 제공하는 테마에서 원하는 <br/>
color이나 style 등을 적용시키는 방법을 알아볼 것이다.
<br/><br/>

## 2 Android Theme
___
### **2.1 기본 Theme** <br/>
```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Theme.TestApp" parent="Theme.MaterialComponents.DayNight.NoActionBar">
        <!-- Primary brand color. -->
        <item name="colorPrimary">@color/purple_500</item>
        <item name="colorPrimaryVariant">@color/purple_700</item>
        <item name="colorOnPrimary">@color/white</item>
        <!-- Secondary brand color. -->
        <item name="colorSecondary">@color/teal_200</item>
        <item name="colorSecondaryVariant">@color/teal_700</item>
        <item name="colorOnSecondary">@color/black</item>
        <!-- Status bar color. -->
        <item name="android:statusBarColor" tools:targetApi="l">?attr/colorPrimaryVariant</item>
        <!-- Customize your theme here. -->

    </style>
</resources>
```
Android 프로젝트를 생성하면 이제는 위와 같은 Theme를 제공한다.<br/>
colorPrimary, textColorPrimary, windowBackground, navigationBarColor 등등<br/>
머티리얼 테마에서 미리 정의가 되어있는 item을 기본 상태로 사용할 수도 있고<br/>
재정의하여 원하는 색으로 적용할 수 있다.<br/><br/>

이것이 가능한 이유는<br/>
안드로이드에서 특정 위치의 색상이 직접 기입된 것이 아니라<br/>
위에서 보이는 테마의 속성을 참조하도록 하였기 때문이다.
<br/><br/>

### **2.2 테마에 해당하는 Color파일 생성하기**<br/>

다크모드 , 라이트 모드 에 해당하는 Color 을 배치하는 방법은 간단하다.
```xml
/res/values
```
기본적으로 color이나 style 파일을 저장하는 values 폴더는 밝은 테마가 기본이며 <br/>
<br/>
```xml
/res/values-night
```
values-night 폴더에 들어가는 같은 이름의 color , style는 다크 테마에 적용된다.
<br/><br/>

### **2.3 테마에 해당하는 Image파일 생성하기** <br/>

```xml
darwable-hdpi
```
위와 같이 기본으로 제공되는 폴더는 밝은 테마에 적용이 되는 이미지이다. <br/>
<br/>
```xml
darwable-night-hdpi
```
darwable-night 폴더에 들어가는 같은 이름의 Image 는 다크 테마에 적용된다. <br/>
<p align="center">
    <img src="https://user-images.githubusercontent.com/52993842/114807402-4d004d80-9de1-11eb-8b08-c15371694a86.png"/>
</p>
위처럼 각 해당하는 폴더에 이미지를 넣으면 테마에 맞춰 적용된다.
<br/><br/>


### **2.4 기본 테마 수정하기** <br/>

```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Theme.TestApp" parent="Theme.MaterialComponents.DayNight.NoActionBar">
       ...

    </style>
</resources>

<resources xmlns:Tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Theme.testApp" parent="Theme.MaterialComponents.DayNight.NoActionBar">
       ...
    </style>
</resources>
```
위처럼 제공하는 테마에서 추가를 하고 싶다면 <br/>
values.xml에서 <br/>
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <attr format="color" name="testColor"/>
</resources>
```

좌측에 리소스 포맷을 적고 사용될 속성 명을 적는다.<br/>
즉 testColor라는 속성은 color을 가지는 리소스이다.<br/><br/>

이제 themes에서 해당 리소스를 추가한다. <br/>

```xml
<!-- Customize your theme here. -->
<item name="testColor">@color/#ff00ff</item>
```

물론 화이트 / 다크 모드에서 적용할 것이기 때문에 두 곳 모두 색에 맞춰 추가한다.<br/>
이제 xml 안에 존재하는 widget에서 사용할 때는<br/>

```xml
android:textColor="?attr/testColor"
```

위와 같이 사용하면 된다.<br/><br/>

attr 은 뒤에 있는 속성 이름으로 값을 찾아오라는 의미이다.<br/>
내가 지금 다크 테마 상태라면 다크 테마에서 해당 속성 이름의 값 찾아오게 되고<br/>
라이트 테마 상태라면 라이트 테마에서 해당 속성 이름의 값을 찾아온다<br/>
<br/><br/>

### **2.4 코드에서 강제로 모드 전환** <br/>

Android의 설정으로 사용자가 테마를 변경하는것이 아닌
앱 자체에서 테마를 전환시킬수있다.
```kotlin
AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_NO) //다크모드 사용안함
AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_YES) // 다크모드

// Android 설정에 맞추기
// 안드로이드 10 이상
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.Q) {
	AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_FOLLOW_SYSTEM);
}
// 안드로이드 10 미만
else {
	AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_AUTO_BATTERY);
}
```

