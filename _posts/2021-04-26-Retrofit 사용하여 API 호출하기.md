---
layout: post
title: Retrofit 사용하여 API 호출하기
author: k4keye
date: 2021-04-26
categories : Android
---
## 1 Retrofit
___
Retrofit는 REST 기반 웹 서비스를 통해 JSON 혹은 그 외의 데이터를 요청하고 <br/>
응답받는 것을 쉽게 하는 라이브러리이다.<br/>
Retrofit는 HTTP 요청을 OkHttp 라이브러리를 사용한다.<br/>

[Retrofit 문서 ](https://square.github.io/retrofit/)

### **1.1 API 확인** <br/>
```kotlin
https://baseURL.co.kr/user/login
post{
	id ="id",
	pwd="pwd"
}
```
위와같은 API를 호출하여

API 성공 응답
```json
{
  "success": true,
  "code": 0,
  "msg": "성공하였습니다.",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJWyJS9VUZXhwIjoxVpUhJBk03uo",
    "status": "A",
    "time": null
  }
}
```

API 실패 응답
```json
{
  "success": false,
  "code": -1002,
  "msg": "아이디 또는 패스워드가 정확하지 않음"
}
```

<br/><br/>

## 2 구현
___
### **2.1 의존성 추가** <br/>
```kotlin
implementation 'com.squareup.retrofit2:retrofit:2.9.0'
implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
```

### **2.2 ApiConstants**<br/>

```kotlin
object ApiConstants {
    const val BASE_URL:String ="https://baseURL.co.kr"
    const val LOGIN_API_PATH:String ="/user/login"
}
```
API 경로를 한곳에서 관리하려고 만든 kt 파일이다.<br/>


### **2.3 RetrofitClient** <br/>
```kotlin
object RetrofitClient {
    private var instance : Retrofit? = null

    fun getInstance() : Retrofit {
        if(instance == null){
            instance = Retrofit.Builder()
                .baseUrl(ApiConstants.BASE_URL) //api 의 기본 uri
                .addConverterFactory(GsonConverterFactory.create())
                .build()
        }
        return instance!!
    }
}
```
Retrofit를 생성하여 사용할 수 있는 Class 파일을 만들어줘야 한다.<bt/>
>
### **2.4 응답 받을 데이터 포멧 준비**<br/>

```kotlin
abstract class ServerResponse {
    val success: Boolean? = null
    val code: Int? = null
    val msg: String? = null
}
```

```kotlin
data class ServerLoginResponse (
    @SerializedName("data")
    val data:Data
): ServerResponse()

data class Data(
    @SerializedName("token")
    val token:String,
    @SerializedName("status")
    val status:String,
    @SerializedName("time")
    val time:String?
)
```

위에서 알아본 성공 실패의 데이터를 기준으로 data class를 만들어줘야 <br/>
api 요청 후 결과를 그대로 받을 수 있다.<br/>



### **2.5 API 인터페이스** <br/>

```kotlin
interface ILoginAPI {

    @POST(ApiConstants.LOGIN_API_PATH)
    fun loginCall(
        @Body json:JsonObject
    ) : Call<ServerLoginResponse>
}
```
@POST 가있는 것처럼 @GET 등 모두 다양하게 제공한다.<br/>
원하는 메서드를 사용하면 되고<br/><br/>

@Body는 JSON 을 보낼 때 사용된다.<br/>
리턴으로 위에서 만든 data class 가있는 것을 볼 수 있다.<br/>
서버의 요청 값을 해당 형태로 받는다.<br/>



### **2.6 LogingAPI 구현**<br/>

```kotlin
class LoginAPI {
    fun login(id:String , pwd:String,completion:(Boolean)-> Unit){
        try{
            var retrofit = RetrofitClient.getInstance()
            var loginApi = retrofit.create(ILoginAPI::class.java)
            val jsonData: JsonObject = JsonObject().apply {
                addProperty("id", id)
                addProperty("pwd", pwd)
            }
            loginApi.loginCall(jsonData).enqueue(object : Callback<ServerLoginResponse> {
                override fun onResponse(
                    call: Call<ServerLoginResponse>,
                    response: Response<ServerLoginResponse>
                ) {
                    if (response.body()!!.success == true) {
                        completion(true)

                    }else{
                        completion(false)
                    }
                }

                override fun onFailure(call: Call<ServerLoginResponse>, t: Throwable) {
                    completion(false)

                }
            })

        }catch (e:Exception){
            completion(false)
        }
    }
}
```

```kotlin
var retrofit = RetrofitClient.getInstance()
var loginApi = retrofit.create(ILoginAPI::class.java)
```

위에서 만든 RetrofitClient에 create() 메서드를 통해  <br/>
위의 Login 인터페이스에서 정의한 엔드 포인트(필요한 자원을 받아올 수 있는 위치)<br/>
POST 메서드와 Login 경로 (/user/login)의 구현체를 만든다.<br/>


### **2.7 API 호출** <br/>
```kotlin
class LoginDataSource {

    fun login(username: String, password: String): Bool {
        try {
            LoginAPI().login(username,password){
                Log.d("결과", "login: it : $it")
                return it
            }
            return false


        } catch (e: Throwable) {
            return false
        }
    }
  
}
```
