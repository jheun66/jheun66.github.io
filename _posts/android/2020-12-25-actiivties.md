---
layout: post
title: Activities
date: 2020-12-25 18:00:00 +0900
categories: [Android]
tags: [Activity]
mermaid: true
---

# 액티비티
액티비티는 안드로이드 앱에서 정말 중요한 **구성요소** 중 하나 입니다. 액티비티는 UI를 포함한 화면이며 이를 통해 사용자와 상호작용을 할 수 있습니다. 하나의 앱에는 여러 액티비티가 있을 수 있으며 서로 독립되어 있습니다. 따라서 액티비티가 실행되고 다른 액티비티와 함께 동작하는 방식을 이해하는 것이 중요합니다.

## 액티비티의 컨셉
C/C++에서 처음 프로그래밍을 접한 사람이라면 **main 함수**를 호출하여 코드를 실행하는 패러다임에 익숙할 것입니다. 하지만 안드로이드 시스템에서는 액티비티 생명 주기의 특정 상태에 해당하는 특정 콜백 메소드를 호출하여 액티비티 인스턴스를 통해 코드를 실행합니다. <br/>
한 앱에서 어떤 액티비티를 진행하다가 다른 액티비티를 실행시키거나 다른 앱을 호출할 때 그 앱을 통째로 호출하는게 아니라 필요한 액티비티를 호출하여 실행시킵니다. 개발자 문서의 예시에서는 이메일 작성 액티비티로 예를 들었습니다. 이메일 앱에서 이메일 작성 액티비티를 호출할 수 있고 카메라 앱에서도 무슨 이메일 버튼을 클리가면 이메일 작성 액티비티에 진입할 수 있을 것 입니다.

![카메라 앱](/assets/img/android_example/KakaoTalk_Photo_2020-12-25-20-47-06.jpeg){: width="240"}

이미지의 빨간 동그라미를 보면 카메라 앱에서는 사진을 찍으면 카카오톡, 인스트그램, 메시지 등의 액티비티와 연결할 수 있고 파란색 동그라미를 보면 이미지 관리 앱의 액티비티와도 연결될 수 있는 것을 확인 할 수 있습니다.

이러한 점에서 액티비티들은 함께 결함하여 앱을 구성하지만 일반적으로 서로에게 최소한의 의존성을 가지고 느슨하게 결합되어 있다고 할 수 있습니다.

## 메니페스트 구성하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapplication">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MyApplication">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```
기본 샘플 _AndroidManifest.xml_ 입니다.

### &lt;activity&gt;
앱에서 액티비티를 사용하기 위해서는 _AndroidManifest.xml_에 선언을 해주어야 합니다.
&lt;application&gt; 태그의 하위 태그로 &lt;activity&gt;를 추가 할 수 있는데 *android:name*은 액티비티 이름으로 반드시 필요한 요소입니다. 그 이외의 요소도 매우 많은데 [&lt;activity&gt;](https://developer.android.com/guide/topics/manifest/activity-element)를 참고하고 추후 직접 예제로 사용할때 마다 정리하도록 하겠습니다.

### &lt;intent-filter&gt;
```xml
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />

        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
```
_action.Main_ 은 이 앱의 메인 진입점을 의미합니다. 따라서 앱을 실행하면 가장 먼저 실행되는 액티비티 입니다.
_category.LAUNCHER_ 는 앱을 실행할 아이콘의 모습이 &lt;activity&gt;에 정의된 것으로 하겠다는 의미입니다. 정의되어 있지 않으면 &lt;application&gt;의 아이콘을 사용합니다.
인텐트와 인텐트 필터에 관한 내용은 중요하다고 판단되어 따로 정리하겠습니다.

### permission
액티비티를 다른 앱에서 실행하거나 못하게 컨트롤 할 수 있습니다. 개발자 예제를 살펴보면
```xml
<manifest>
<activity android:name="...."
   android:permission=”com.google.socialapp.permission.SHARE_POST”

/>
```
다음과 같이 정의하면 이 액티비티를 호출하기 위해서는 `com.google.socialapp.permission.SHARE_POST`를 사용한다고 선언해야 합니다. 
```xml
<manifest>
   <uses-permission android:name="com.google.socialapp.permission.SHARE_POST" />
</manifest>
```
이때는 &lt;manifest&gt;에 &lt;uses-permission&gt;으로 같은 권한을 정의합니다.

이와 마찬가지로 안드로이드 기기의 다양한 기능들을 활용하기 위해서는 안드로이드 API에 정의된 권한들을 선언해야합니다.<br/>
[*Manifest.permission*](https://developer.android.com/reference/android/Manifest.permission)

## Activity LifeCycle
[**이어서**](/posts/activity-lifecycle)

## Reference
[**안드로이드 개발자 사이트**](https://developer.android.com/guide/components/activities/intro-activities)