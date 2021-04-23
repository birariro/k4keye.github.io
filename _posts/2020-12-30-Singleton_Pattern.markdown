---
layout: post
title: Singleton Pattern
author: k4keye
date: 2020-12-30
categories : DesignPattern
---
<br/>
<br/>

# 1 Singleton Pattern
___
# **1.1 Singleton Pattern(싱글톤 패턴)**
<br/>
싱글톤 패턴이란 하나의 인스턴스를 재활용하여 사용하게 되는 패턴이다.<br/>
하나의 기능 Class를 사용하기 위해 new를 사용하여 새로운 인스턴스를 매번 생성한다면<br/>
그것은 프로그램에 있어 매우 부담스러운 일일 수 있다.<br/><br/>
이때 Class의 역할이 공유 변수 없이 단순하게 사용되는 역할을 수행한다면<br/>
매번 생성하지 않고 **하나의 인스턴스를 공유**하여 사용할 수 있다.<br/><br/>

# **1.2 요구 사항**
<br/>
<p align="center">
    <img src ="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDzESI%2FbtqPsDiv26C%2F7MWzMnuitruZFaprszIC51%2Fimg.png"/>
</p>
2개의 로직이 API라는 하나의 로직을 사용해 아 하는 요구 사항이다.<br/><br/>

# **1.3 기존 코드**
<br/>
이때 싱글 톤을 적용하지 않았다고 한다면<br/>

```java
Class 기능1
{
    Api api = new Api();
    api.기능();
}
Class 기능2
{
    Api api = new Api();
    api.기능();
}
```
위와 같이 2개의 기능에서 API의 인스턴스를 매번 생성해 줘야만 한다.
<br/>
<br/>

# 2. 실습
___
```java
public class Api
{
	void Get()
	{

	}
	void Post()
	{

	}
}
```
<br/>
위와 같이 API Class의 사용되는 메서드가 2개 있다고 예를 보자.<br/>
이때 2개의 로직에서 해당 Class를 사용할 때 매번 인스턴스를 생성하지 않게<br/>
Singleton Pattern 을 적용시켜보자.<br/>
<br/>

```java
public class Api
{
	static private Api instance;

	private Api()
	{

	}
	public static Api GetInstance()
	{
		if(instance ==null)
		{
			instance = new Api();
		}
		return instance;
	}
	void Get()
	{

	}
	void Post()
	{

	}
}
```
<br/>
 
위와 같이 적용시킬 수 있다.<br/>

해당 Class의 인스턴스를 static로 생성시키고<br/>
해당 Class를 사용할 때에는 GetInstance() 메서드를 사용하여<br/>

인스턴스를 받아 사용하게 된다.<br/><br/>

 
이로 인해
```java
Api api = new Api();
```
<br/>
위와 같이 새로운 인스턴스를 생성하지 못하게 되고<br/>

```java
Api api = Api.GetInstance();
```
<br/>
위와 같이 인스턴스를 받아서 사용할 수밖에 없게 된다.<br/><br/>

## **2.1 예제 코드**
<br/>

[예제 코드 Git 주소 ](https://github.com/k4keye/DesignPattern)