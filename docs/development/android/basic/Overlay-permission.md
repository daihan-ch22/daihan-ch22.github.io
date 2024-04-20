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

## reference
https://stickode.tistory.com/158
