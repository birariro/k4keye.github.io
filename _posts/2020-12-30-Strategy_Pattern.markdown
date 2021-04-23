---
layout: post
title: Strategy Pattern
author: k4keye
date: 2020-12-30
categories : DesignPattern
---
<br/>
<br/>

# 1 Strategy Pattern
___

## **1.1 Strategy Pattern(전략 패턴)**
<br/>
객체들이 할 수 있는 행위들을 클래스로 작성하고 행위를 자유롭게 바꿀 수 있게 하는 패턴으로 <br/>

같은 역할을 하는 여러 알고리즘을 여러 클래스로 작성하고 이를 필요할 때 교체하여 <br/>
사용할 수 있게 하는 디자인 패턴이다.<br/><br/>

**즉 전체적으로는 같은 동작을 하는 여러 일을 전략적으로 나누어서 쉽게 교체할 수 있게 하는 패턴이다.**<br/><br/>



## **1.2 요구 사항**
<br/>

<p align="center">
    <img src ="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FzfkeC%2FbtqPh98sLw0%2FO3q8N11w3EHREzmx0nJq80%2Fimg.png"/>
</p>

위와 같이 각각의 로직들이 필요로 하는 기능들이<br/>

각각의 역할을 수행할 수 있는 알고리즘을 사용해야 할 때<br/>

이 알고리즘을 인터페이스로 정의하여 동적으로 바꿔서 사용하게 할 수 있다.<br/><br/>

<p align="center">
    <img src ="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlQxkx%2FbtqPfTZlbM1%2FPFzO7hS3feFfB16xQQH7R0%2Fimg.png"/>
</p>


# 2. 실습 
___

## **2.1 코드 구현**
<br/>
먼저 필요한 것은 모든 로직이 사용할 API를 구현하기 전에<br/>

API에서 전략적으로 바뀔 클래스를 정의할 인터페이스가 먼저 필요하다.<br/>

```java
public interface IAPI
{
	void Get();
	void Post();
}
```
<br/>

모든 인터페이스는 Get 과 Post를 사용해야 한다고 보고 위와 같이 구성하였다.<br/>

그리고 각각의 UserAPI , CheckAPI , SyncAPI에서 위의 인터페이스를 구현한다.<br/>

``` java
class UserAPI : IAPI
{
	public void Get()
	{
		Console.WriteLine("User API GET 호출");
	}

	public void Post()
	{
		Console.WriteLine("User API POST 호출");
	}
}

class CheckAPI : IAPI
{
	public void Get()
	{
		Console.WriteLine("Check API GET 호출");
	}

	public void Post()
	{
		Console.WriteLine("Check API POST 호출");
	}
}

class SyncAPI : IAPI
{
	public void Get()
	{
		Console.WriteLine("Sync API GET 호출");
	}

	public void Post()
	{
		Console.WriteLine("Sync API POST 호출");
	}
}
```
<br/>
지금은 간단하게 호출된 로직을 화면에 출력하는 게 끝이다.<br/>

이제 모든 로직들이 요청할 API Class를 구현할 것이다.<br/>
```java
public class API
{
	private IRequest request;
	public void setAPI(IRequest request)
	{
		this.request = request;
	}
	public void Get()
	{
		request.Get();
	}
	public void Post()
	{
		request.Post();
	}
}
```
<br/>
이곳에서 중요한 부분이 바로 <br/>

```java
private IRequest request;
public void setAPI(IRequest request)
{
	this.request = request;
}
```
<br/>
이 부분인데 해당 API 클래스를 사용할 때는 인터페이스를 구현한 클래스 중 <br/>
어느 클래스로 사용할지를 받게 한다.<br/><br/>


이렇게 만들어진 전략 패턴을 사용해보자.<br/>

API를 필요로 하는 모든 곳은 API class를 통해 사용해야 한다.<br/>

```java
API api = new API();
```
<br/>
그 후 API 중 어느 API로 갈지를 인스턴스로 넣어줘야 한다.

```java
API api = new API();

api.setAPI(new SyncAPI());
api.Get();
api.setAPI(new UserAPI());
api.Post();
api.setAPI(new SyncAPI());
api.Get();
```
<br/>
<p align="center">
    <img src ="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FvWNry%2FbtqPnbYYSQW%2FSn7Nogk3mhDUrN0hQJKmK1%2Fimg.png"/>
</p>

이제 새로운 API가 나오더라도 API를 호출하는 방법은 똑같지만<br/>

setAPI()에서 새로운 API를 추가해서 사용할 수 있게 된다.<br/>
<br/>

## **2.2 예제 코드**
<br/>

[예제 코드 Git 주소 ](https://github.com/k4keye/DesignPattern)