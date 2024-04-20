---
title: Requesting overlay permission
created: 2023-12-16
categories:
- Android
---

# Android - Overlay View 권한 체크 & 띄우고 제거하기

## Manifest
```xml
<uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>
<uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>
```


## 권한 체크

```kotlin
if (Settings.canDrawOverlays(this)) {
    //오버레이 가능한 상태
} else {
    //오버레이 불가능한 상태 -> 권한 요청 
}
```

## 권한 요청

```kotlin
fun overlayPermission() {
    val uri = Uri.parse("package:${packageName}")
    val intent = Intent(Settings.ACTION_MANAGE_OVERLAY_PERMISSION, uri).addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP)
    overlayLauncher.launch(intent) //런쳐 호출 
}
```

## 권한 요청 콜백

```kotlin
val overlayLauncher =
    registerForActivityResult(ActivityResultContracts.StartActivityForResult()) {
        if (Settings.canDrawOverlays(this)) {
            // 오버레이 가능한 상태 
            // 다음 로직 진행 
        } else {
            // 오버레이 권한이 없으니 다시 권한 요청 (강제인 경우)
            overlayPermission()
        }
    }
```

## 오버레이 띄우기

```kotlin
fun initOverlay() {
    val layoutInflater = getSystemService(Context.LAYOUT_INFLATER_SERVICE) as LayoutInflater
    val view = layoutInflater.inflate(R.layout.overlayview, null)
    val wm = getSystemService(WINDOW_SERVICE) as WindowManager

    val params = WindowManager.LayoutParams(
        ViewGroup.LayoutParams.WRAP_CONTENT,
        ViewGroup.LayoutParams.WRAP_CONTENT,
        100,
        100,
        TYPE_APPLICATION_OVERLAY,
        WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE,
        PixelFormat.TRANSLUCENT
    )
    params.gravity = Gravity.TOP or Gravity.CENTER

    wm.addView(view, params)
}
```

## 오버레이 없애기

```kotlin
fun removeOverlay() {
    val layoutInflater = getSystemService(Context.LAYOUT_INFLATER_SERVICE) as LayoutInflater
    val view = layoutInflater.inflate(R.layout.overlayview, null)
    val wm = getSystemService(WINDOW_SERVICE) as WindowManager

    wm.removeView(view)
}
```

---

## 삼성 단말기에서는 통화중 사용 불가

!!! warn
    **삼성 단말기(갤럭시 기기)에서 공식 경로(Google Play & One Store 등)가 아닌 방법으로 apk를 설치 할 경우<br>
    보안 관련 메시지가 나온다.**

다만 모든 OS에서 뜨는건 아니고 Android13(One UI 5.0)이상에서 발생하는데 "무시하고 설치"하게되면
전화 통화 시 Overlay 기능이 차단된다. (SYSTEM_ALERT_WINDOW)

삼성 보안 정책이기 떄문에 MDM처럼 기기 관리 앱을 통해 설치하지 않는 이상, 해당 기능을 사용할 수 없을것으로 보인다.

Settings.canDrawOverlays를 통해 오버레이 가능 여부를 확인해보면 권한을 부여했음에도 전화 통화 시 가능 여부가 불가능으로 변경된다.

```kotlin
Settings.canDrawOverlays(this)
```

!!! info
    기능 사용이 꼭 필요한 경우, 보통의 방법으로 3가지가 있을 것 같다.

    1. 공식 스토어를 통해 배포 
    2. ADB로 밀어넣기 (삼성에서 개발을 위해 허용한 방법)
    3. Android 12이하 기기로 설치

---

## reference
https://stickode.tistory.com/158  
https://forum.developer.samsung.com/t/android13-system-alert-window-permission-not-checked/24454

