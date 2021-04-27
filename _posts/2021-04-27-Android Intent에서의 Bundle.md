---
layout: post
title: Android Intent에서의 Bundle
author: k4keye
date: 2021-04-27
categories : Android
---
## 1 Intent
___
인텐트는 메시징 객체로 다른 요소로부터 작업을 요청하는 데 사용할 수 있다.<br/>
대표적으로 사용하는 방법이 역시 액티비티를 호출하는 것이다.<br/>


## 2 의문
___

### **2.1 Intent 에서 데이터 전달**<br/>

intent에서 데이터를 전달하는 방법으로 인텐트 내부에 구현되어 있는 putExtra 메서드를 활용하는 방법이 있다.<br/>
여기서 의문점이 드는데<br/>
```kotlin
val intent = Intent()
intent.putExtra("key","value")
```

```kotlin
val intent = Intent()

val bundle =Bundle()
bundle.putString("key","value")
intent.putExtra("bundle",bundle)
```
이 두 차 이를 알고 싶다<br/>
위와 같이 두 방식을 사용하게 돼도 <br/>
같은 결과가 이루어지게 되는데 <br/><br/>

한눈에 봐도 Bundle를 사용하는 부분이 더 손이 많이 가고 번거로움을 알 수 있다.<br/>
따라서 Bundle를 사용하지 않는 게 더 편해 보이긴 한다.
<br/><br/>

### **2.2 내부 구조** <br/>
편한 건 Bundle를 사용하지 않는 것이 분명 편한 건 있다.<br/>
하지만 내부 구조를 보면 말이 달라진다.<br/>

```kotlin
private Bundle mExtras;
public @NonNull Intent putExtra(String name, @Nullable String value) {
	if (mExtras == null) {
		mExtras = new Bundle();
	}
	mExtras.putString(name, value);
	return this;
}
```
Intent의 putExtra()는 결국 Bundle를 사용하고 있다.<br/>
즉 putExtra()는 단순히 편리함을 위해 도와주고 있는 모습으로 보인다.<br/>
<br/>
그에 반해

```kotlin
ArrayMap<String, Object> mMap = null;
public void putString(@Nullable String key, @Nullable String value) {
	unparcel();
	mMap.put(key, value);
}
```
Bundle의 메서드를 확인해보면 Map를 활용하여 데이터를 넣고 있다.<br/><br/>


### **2.3 결론**<br/>

그럼 내부적으로는 Bundle를 쓰고 있느니 <br/>
그냥 편리한 Intent의 메서드들을 활용하는 게 좋은 걸까<br/>
<br/>
문제는<br/>
```kotlin
if (mExtras == null) {
	mExtras = new Bundle();
}
```
위의 코드가 마음에 걸린다.<br/>
데이터를 넣을 때마다 조건문을 한번 통과를 해야 하는 것인데.<br/>
<br/>
그에 반해 Bundle는<br/>
```kotlin
mMap.put(key, value);
```
Map에 바로 put 하기 때문에<br/>
데이터를 넣는 양에 따라 속도 차이가 날... 수도 있겠다.<br/>
<br/>
결론적으로 Bundle를 활용하는 게 더 좋아 보인다.<br/>