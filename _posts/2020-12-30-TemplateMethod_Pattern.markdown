---
layout: post
title: TemplateMethod Pattern
author: k4keye
date: 2020-12-30
categories : DesignPattern
---
<br/>
<br/>

# 1 TemplateMethod Pattern
___
## **1.1 TemplateMethod Pattern(탬플릿 메소드 패턴)**
<br/>
어떤 작업을 처리하는 일련의 프로세스를 정의해두고<br/>
서브 클래스가 프로세스에 필요한 역할을 구현하여<br/>
요청하는 입장에서는 프로세스를 호출만을 하게 하여<br/>
프로세스의 변동이 생겨도 요청하는 입장에서는 변동이 없게 하는 패턴이다.<br/>
​
동일한 기능은 상위 클래스에서 정의하며 확장 혹은 변화가 발생하는 부분만 서브 클래스에 
<br/>구현한다.<br/>

여기서 프로세스라는 것을 무엇을 의미할까?<br/>

예로들어 크롤링이라는 주제로 코드를 작성하게된다면 하나의 작업을 수행하는것이아닌<br/>
 여러 작업을 수행하여 결과를 도출하게될것이다.<br/>

1). 사이트에 접속한다. 

2). 해당 사이트에서 HTML 코드를 가지고온다.

3). 필요로 하는 부분의 문자를 잘라낸다.<br/>

위와같은 작업을 수행이 모두 마쳐야만 크롤링이라는 작업이 끝나는것일것이다. 여기서 크롤링 작업이 하나의 프로세스이다.<br/><br/>

## **1.2 요구 사항**
<br/>
위에서는 크롤링을 예로 들었지만 이번에는 로그인이라는 하나의 프로세스를 기준으로 봐보자
<br/><br/>

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlXmu2%2FbtqPgGS2gjA%2FkakLwnxGDPvdTNqVl41Lk1%2Fimg.png" >
</p> 

위와 같이 로그인을 시도하는 일련의 프로세스가 필요하다고 <br/>
보았을 때 로그인에 필요한 동작들이 존재한다.<br/>
이것이 로그인의 프로세스이다.<br/><br/>

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbCQ2Ri%2FbtqPaWbHzON%2FFAQLYXXVkA82mMC6LKCQH0%2Fimg.png" >
</p> 
<br/>
위의 요구 사항을 바탕으로 로그인이라는 템플릿을 두어 <br/>
로그인 요청 로직은 프로세스 자체를 호출시키기만 하게 한다.


# 2. 실습
___

## **2.1 코드 구현**
<br/>
로그인이라는 하나의 프로세스를 추상 클래스로 구현하였다.<br/>

``` java
public abstract class LoginConnect
{
	protected abstract string PwdSecurity(string pwd); //패스워드 암호화
	protected abstract string Connect(string id ,string pwd); //로그인 시도
	protected abstract string Auth(string userInfo); //권한 확인

	public string rquestConnection(string id , string pwd)
	{
		string SecurityPwd = PwdSecurity(pwd);
		string userInfo = Connect(id, SecurityPwd);
		string userAuth = Auth(userInfo);
		string result = "";
		switch (userAuth)
		{
			case "일반":
				result = "일반회원";
				break;
			case "관리자":
				result = "관리자";
				break;
		}
		return result;
	}

}
```

위의 코드중<br/>
```java
protected abstract string PwdSecurity(string pwd); //패스워드 암호화
protected abstract string Connect(string id ,string pwd); //로그인 시도
protected abstract string Auth(string userInfo); //권한 확인
```
이 부분은 서브 클래스가 구현하게 되고<br/>

```java
public string rquestConnection(string id , string pwd)
```
이 부분은 로그인 프로세스가 필요한 로직이 호출하게 한다.<br/>

```java
class NormalLogin : LoginConnect
{
	protected override string PwdSecurity(string pwd)
	{
		Console.WriteLine("패스워드 암호화 작업");
		return pwd;
	}
	protected override string Connect(string id, string pwd)
	{
		Console.WriteLine("로그인 시도후 로그인 정보 반환");
		return "userinfoAdmin";
	}
	protected override string Auth(string userInfo)
	{
		Console.WriteLine("로그인 정보를 통해 권한 반환");
		return "관리자";
	}

}
```
서브 클래스를 두어 프로세스에 필요한 각 로직을 구현한다.<br/>

이렇게 되면<br/>

프로세스를 호출하는 쪽에서는<br/>
rquestConnection() 메서드만을 호출하게 되고<br/>
나머지 로그인에 필요한 작업은 서브 클래스가 위임하여 처리하게 된다.<br/>

즉 호출하는 쪽에서는 이제 로그인 프로세스가 확장 혹은 변화가 생기더라도 호출하는 방법을 바꿀 필요가 없다.<br/>

```java
LoginConnect loginConnect = new NormalLogin();
Console.WriteLine(loginConnect.rquestConnection("id", "pwd"));
```
<br/>

<p align="center">
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Flc4On%2FbtqPnaltdN7%2F7w67R8WGnpuMjVOgfZ72CK%2Fimg.png">
</p>


## **2.2 예제 코드**
<br/>

[예제 코드 Git 주소](https://github.com/k4keye/DesignPattern)