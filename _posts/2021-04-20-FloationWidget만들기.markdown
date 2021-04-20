---
layout: post
title: Android FloationWidget 구현하기
author: k4keye
date: 2021-04-20
categories : Android
---
<br/>
<br/>

# 요구사항
___
앱 밖에 표시되는 즉 안드로이드 디바이스 화면 에 표시되는 view를 만들고싶다.
 <br/>

# 구현 방법
___
1. 화면위에 그리기 원한을 사용한다.
2. 서비스에서 view를 호출한다.

 <br/>

# 구현
___

<br/>

## 1. 권한

```kotlin
<uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />
<uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
```
화면위에 그리기 권한과 <br/>
안드로이드 오레오 이상에서 서비스를 호출하기위한 포그라운드 서비스 등록
 <br/> <br/>
 
## 2. 권한획득 및 서비스 호출
```kotlin
class MainActivity : AppCompatActivity() {
    private val serviceIntent by lazy {  Intent(this, ImmortalService::class.java)  }
    companion object {
        private const val ACTION_MANAGE_OVERLAY_PERMISSION_REQUEST_CODE = 1004
    }

    @RequiresApi(Build.VERSION_CODES.M)
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        checkPermission()
    }
    @RequiresApi(Build.VERSION_CODES.M)
    private fun checkPermission() {

        when{
            Settings.canDrawOverlays(this) -> {
                callService()
            }
            else->{
                permissionPopup()
            }
        }

    }
    @RequiresApi(Build.VERSION_CODES.M)
    private fun permissionPopup(){
        AlertDialog.Builder(this@MainActivity)
                .setTitle("권한이 필요합니다.")
                .setMessage("화면위의 그리기 권한이 필요")
                .setPositiveButton("확인") { _, _ ->
                    val intent = Intent(Settings.ACTION_MANAGE_OVERLAY_PERMISSION, 
                                        Uri.parse("package:$packageName"))
                    startActivityForResult(intent, 
                                        ACTION_MANAGE_OVERLAY_PERMISSION_REQUEST_CODE)
                }
                .setNegativeButton("취소") { _, _ -> }
                .create().show()

    }
    private fun callService() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            startForegroundService(serviceIntent)
        } else {
            startService(serviceIntent)
        }
    }
    @RequiresApi(api = Build.VERSION_CODES.M)
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        if (requestCode == ACTION_MANAGE_OVERLAY_PERMISSION_REQUEST_CODE) {
            callService()
        }
    }
}
```
MainActivity에서는 화면위에 그리기 권한과 서비스 호출을 하고있다.
 <br/> <br/>
## 3. 서비스 구현
```kotlin
class ImmortalService : Service() {

    override fun onBind(intent: Intent): IBinder? {
        return null
    }

    override fun onCreate() {
        super.onCreate()
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            val channelID = "채널id"
            val strTitle = "타이틀"

            val notificationManager = getSystemService(NOTIFICATION_SERVICE) as NotificationManager
            var channel = notificationManager.getNotificationChannel(channelID)
            var builder: NotificationCompat.Builder? = null
            var notification: Notification? = null
            if (channel == null) {
                channel = NotificationChannel(
                    channelID,
                    strTitle,
                    NotificationManager.IMPORTANCE_HIGH
                )
                notificationManager.createNotificationChannel(channel)
            }
            builder = NotificationCompat.Builder(this, channelID)
            builder.setSmallIcon(R.mipmap.ic_launcher)
            builder.setContentText("노티 텍스트")
            notification = builder.build()
            notificationManager.notify(1, notification)
            startForeground(1, notification)
        }
        initView()
    }

    fun initView(){
        // inflater 를 사용하여 layout 을 가져오자
        val inflate = getSystemService(Context.LAYOUT_INFLATER_SERVICE) as LayoutInflater
        // 윈도우매니저 설정
        val windowManager = getSystemService(WINDOW_SERVICE) as WindowManager
        val params = WindowManager.LayoutParams(
            WindowManager.LayoutParams.WRAP_CONTENT,
            WindowManager.LayoutParams.WRAP_CONTENT,
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) WindowManager.LayoutParams.TYPE_APPLICATION_OVERLAY 
            else WindowManager.LayoutParams.TYPE_SYSTEM_ALERT,  // Android O 이상인 경우 TYPE_APPLICATION_OVERLAY 로 설정
            
            WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE
                    or WindowManager.LayoutParams.FLAG_NOT_TOUCH_MODAL or WindowManager.LayoutParams.FLAG_WATCH_OUTSIDE_TOUCH,
            PixelFormat.TRANSLUCENT
        )
        // 위치 지정
        params.gravity = Gravity.LEFT or Gravity.CENTER_VERTICAL

        // activity_overlay.xml 불러오기
        val mView = inflate.inflate(R.layout.activity_overlay,null)
        val overlayButton :Button = mView.findViewById(R.id.btn)
        overlayButton.setOnClickListener{
            Log.d("오버레이", "initView: 버튼 클릭")
        }
        windowManager.addView(mView,params)

    }

}
```
해당 서비스의 역활은 죽지않는 서비스를 위한 Notification channel 생성과 <br/>
xml 파일을 WindowManager에게 적용시켜 <br/>
앱 밖에 표시되도록 한다. <br/>
 <br/>
## 4. 앱 밖에 표시될 화면
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    xmlns:android="http://schemas.android.com/apk/res/android">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btn"
        android:text="click"
        android:textColor="#ffffff"
        android:layout_marginTop="12dp" />

</LinearLayout>
```
 <br/> <br/>
# 결과
___
![image](https://user-images.githubusercontent.com/52993842/115334969-47d34200-a1d7-11eb-9dff-8f6d0804a906.png)
![image](https://user-images.githubusercontent.com/52993842/115334983-4dc92300-a1d7-11eb-8608-c20a9b6cc2c6.png)

