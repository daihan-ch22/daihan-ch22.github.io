---
title: EncryptedSharedPreferences
slug: encryptedSharedPreferences
categories:
  - Android
---

기존에 SharedPreferences를 통해 사용자 아이디나 토큰값 같은 기타 정보를 저장하고 있었는데 그냥 쓰게되면 값이 그대로 노출되기 때문에 보안상 취약하다.
/data/data/앱 패키지/shared_pref로 가보면 preference가 적혀있는 파일 확인이 가능한데 아래처럼 평문으로 보인다.

### 기존
```xml
<?xml version='1.0' encoding='utf-8' standalone='yes' ?>
<map>
    <string name="token">1q2w3e4r</string>
</map>
```
위처럼 토큰값이 그대로 노출되고 있다. 이를 보완하기 위해 EncryptedSharedPreferences를 사용할 수 있다.

<!-- more -->


### Dependency 
```kotlin
implementation("androidx.security:security-crypto:1.1.0-alpha06")
```

### MasterKey
```kotlin
val masterKey = MasterKey
            .Builder(MyApplication.applicationContext(), MasterKey.DEFAULT_MASTER_KEY_ALIAS)
            .setKeyScheme(MasterKey.KeyScheme.AES256_GCM)
            .build()
```

### EncryptedSharedPreference
```kotlin
val encPref = EncryptedSharedPreferences.create(
    MyApplication.applicationContext(),
    "enc_pref", //pref 파일 이름
    masterKey,
    EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
    EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM
)
```

### 결과
```xml
<?xml version='1.0' encoding='utf-8' standalone='yes' ?>
<map>
    <string name="__androidx_security_crypto_encrypted_prefs_key_keyset__">12a90147ef3542f544cfa45c6eaf27712a1baf2a462e897b6b4036f898d047a3adcef4968101046a80bfced2ed0d48c9cfd75c32ccaeb8d429b0c3bc9eeb19e6c84705b1a3904adf1a5866dd4f59c83bd2214dfabdea0cd49827378db9be394a4436feac5d6845758f79f9c85cff8fd5694b96ecf6452d0ad5c3504ad27082199c310c9c128af770efa6a468c256ef1e73735e3efe71af3342b869cb68dd40a87f93a7c90a0dc94d4c5cf6b11a4408e7cfda8507123c0a30747970652e676f6f676c65617069732e636f6d2f676f6f676c652e63727970746f2e74696e6b2e4165735369764b6579100118e7cfda85072001</string>
    <string name="AXC2p+eL4qLeDnwI6AVRUutqeaicUw9Ic+P2Wks=">ASqTMC6kq+pWwpAYo9W1n87PewIYq9Qj/cbrrhRq3vZboIhvxYgKREepubtP/REQsg==</string>
    <string name="__androidx_security_crypto_encrypted_prefs_value_keyset__">128801704c2dc021a663051f11822f70008af4abed70156d5a3da10e4e757e5e7bcf066316d7ce015c5a10ed0b352b41307ccabc9dd3a1d5b78bca72cc6550ed596ad2b03994bd79ffc046fa2da41af8ff9262f1f8afd01987756269434518b9638627eddf0c5e6d3829e9ccb83ef8417bb122bf9ac31eaeef01e8a243dbf43662cd10b737d42711715b511a4408aee0ccd402123c0a30747970652e676f6f676c65617069732e636f6d2f676f6f676c652e63727970746f2e74696e6b2e41657347636d4b6579100118aee0ccd4022001</string>
</map>
```
결과를 보면 기존처럼 똑같이 preference 파일은 추가되지만 내용은 암호화 되어 알 수 없게 된다
