---
layout: post
title: Implict Intent
date: 2021-01-10 03:00:00 +0900
categories: [Android]
tags: [Intent, Implict Intent]
---

[**Intent**](/posts/intent)에 이어서 작성하겠습니다.

Explict Intent에서는 클래스명으로 호출하여 다른 액티비티를 호출했습니다. 하지만 Implict Intent를 이용한 호출 방법은 조금 다릅니다.
특정 클래스 명을 지정하지 않더라도 원하는 액션과 특정 데이터들을 이용하여 실행할 수 있는 액티비티, 다른 컴포넌트들을 호출할 수 있습니다. 매칭된 액티비티가 여러 개일 경우 사용자가 선택할 수 있습니다.

> Intent가 무엇을 실행시킬지 *결정* 하는 것을 Intent Resolution이라 합니다. 만일 기기에 인텐트를 처리할 수 있는 앱이 존재하지 않으면 비정상 종료됩니다. 

# Intent Infomation
## Action
다른 액티비티가 받을 일반적인 액션을 정합니다. ACTION_VIEW, ACTION_SEND 등 ACTION_ 가 붙습니다. 
[Standard Activity Action](https://developer.android.com/reference/android/content/Intent#standard-activity-actions)를 확인해보면 알 수 있듯이 Intent클래스에 상수로 포함되어 있습니다. 브로드 캐스트와 관련된 Action도 있습니다.

## Data
Explict Intent에서와 마찬가지로 데이터를 보낼 수 있습니다. 주로 브라우저에 URI를 넣어 특정 사이트를 호출하거나 할 수 있습니다.
type도 마찬가지로 setType()으로 세팅 가능합니다.

## Category
카테고리 항목은 옵션입니다. 액션만을 이용하서 고르는 것보다 더 세세하게 고를 수 있습니다. 카테고리는 여러 개가 들어갈 수 있습니다. 
[Standard Category](https://developer.android.com/reference/android/content/Intent.html#standard-categories)에서 주로 사용되는 카테고리를 확인 할 수 있습니다.

# Send & Receive
## startActivity or startActivityforResult
Explict Intent에서도 사용했던 방법으로 해당 방법으로 액티비티를 호출하기전에 `intent.resolveActivity(getPackageManager())`를 이용하여 해당 액티비티를 실행할 수 있는지 확인할 수 있습니다.
> 만일 기기에 인텐트를 처리할 수 있는 앱이 존재하지 않으면 비정상 종료됩니다.

## ShareCompat.IntentBuilder
공유하기에 관련된 [헬퍼](https://developer.android.com/reference/androidx/core/app/ShareCompat.IntentBuilder)입니다. 해당 클래스의 메소드를 이용하여 추가적으로 세팅할 수 있습니다.

## Receive Implict Intent
액티비티에서 Implict Intent를 받기위해서는 Intent Filter가 필요합니다. _AndroidMenifest.xml_ 에 정의를 해줄 수 있습니다. 만일 &lt;intent-filter&gt;항목이 없다면 Explict Intent로만 호출이 가능합니다. 여기서도 마찬가지로 액티비티가 받을 action, catagory, data들을 정의해 줄 수 있습니다.

``` xml
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    <data android:scheme="http" android:host="developer.android.com" />
</intent-filter>
```
예시로 해당 파트의 실습에서 사용한 코드입니다. 이 액티비티는 ACTION_VIEW 액션에 default(기본)나 browsable 카테고리, http Scheme에 "developer.android.com"에 대해서만 처리를 해줍니다.
host를 설정하지 않으면 모든 http 스킴에 대해서 처리해줄 수 있습니다. 만일 https에 대해서도 받아주고 싶다면 https Scheme을 추가해 주면 됩니다. 그리고 default는 startActivity(), startActivityForResult()에 포함이 되어 있어 Implict Intent를 수신하는 경우 반드시 포함되어야 합니다. 

## Reference
<https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/unit-1-get-started/lesson-2-activities-and-intents/2-3-c-implicit-intents/2-3-c-implicit-intents.html><br>
<https://developer.android.com/codelabs/android-training-activity-with-implicit-intent#0><br>
<https://developer.android.com/reference/android/content/Intent.html><br>
<https://developer.android.com/reference/androidx/core/app/ShareCompat.IntentBuilder>

