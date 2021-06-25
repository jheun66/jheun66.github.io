---
layout: post
title: resolveActivity VS queryIntentActivities
date: 2021-01-13 14:30:00 +0900
categories: [Android]
tags: [Activity, Intent]
---

# 개요
_startActivity(intent)_ 를 실행하기 전 해당 intent를 실행시킬 수 있는지 체크할 필요가 있습니다. 만일 기기에 인텐트를 처리할 수 있는 앱이 존재하지 않으면 비정상 종료되기 때문입니다.

이 때 사용하는 방법에 _resolveActivity_ , _queryIntentActivities_ 가 있습니다.

## resolveActivity()

``` java
String url = mWebsiteEditText.getText().toString();
Uri webpage = Uri.parse(url);
Intent intent = new Intent(Intent.ACTION_VIEW, webpage);
if (intent.resolveActivity(getPackageManager()) != null) {
    startActivity(intent);
} else {
    Log.d("openWebSite()", "Can't handle this!");
}
```
이전 예제에서 사용한 코드입니다.

개발자 문서를 찾아보면 다음과 같은 설명이 있습니다.

><b>public ComponentName resolveActivity (PackageManager pm)</b>

>Return the Activity component that should be used to handle this intent. The appropriate component is determined based on the information in the intent, evaluated as follows:

해당 인텐트를 핸들링할 수 있는 액티비티 컴포넌트를 리턴합니다. 적절한 컴포넌트는 인텐트의 정보들을 기반으로 다음을 평가로 결정됩니다.

>If getComponent() returns an explicit class, that is returned without any further consideration.

getComponent()의 리턴 값이 명시적 클래스일 경우, 즉 Explict Intent일 경우 다른 고려를 하지 않고 해당 값을 반환합니다.

>The activity must handle the Intent#CATEGORY_DEFAULT Intent category to be considered.

액티비티는 CATEGORY_DEFAULT가 반드시 핸들링 되어야합니다. 이건 이전 포스팅에서 확인한 &lt;intent-filter&gt;에 `&lt;category android:name="android.intent.category.DEFAULT" /&lt;`가 포함되어 있어야 한다는 것을 의미합니다.

>If getAction() is non-NULL, the activity must handle this action.

인텐트에 액션이 있으면 액티비티는 반드시 해당 액션을 핸들링해야 합니다.

>If resolveType(ContentResolver) returns non-NULL, the activity must handle this type.

인텐트에 타입이 있으면 액티비티는 반드시 해당 타입을 핸들링해야 합니다.

>If addCategory(String) has added any categories, the activity must handle ALL of the categories specified.

마찬가지로 카테고리가 추가되어 있으면 해당 카테고리를 핸들링해야 합니다. 

>If getPackage() is non-NULL, only activity components in that application package will be considered.

패키지가 있으면 오직 해당 어플리케이션 패키지에 포함되 액티비티 컴포넌트만 고려되어야 합니다.

>If there are no activities that satisfy all of these conditions, a null string is returned.

만족하는 액티비티가 없을 경우 빈 문자열이 리턴됩니다.

>If multiple activities are found to satisfy the intent, the one with the highest priority will be used. If there are multiple activities with the same priority, the system will either pick the best activity based on user preference, or resolve to a system class that will allow the user to pick an activity and forward from there.

만일 여러 액티비티가 인텐트 조건을 만족하면 가장 우선순위가 높은 액티비티가 반환됩니다. 만일 우선순위가 같다면 시스템은 사용자가 선호하는 액티비티를 선택할 수 있도록 하고 그 후에 사용자의 선호를 따를 수 있도록 합니다. 

![select activity](/assets/img/android_example/selectActivity.jpg)

이는 저희가 흔히 본 앱 선택창을 말합니다.


>This method is implemented simply by calling PackageManager#resolveActivity with the "defaultOnly" parameter true.

이 메소드는 PackageManager의 resolveActivity에 매개변수로 defaultOnly가 true로 호출함으로써 구현되어 있습니다.

이를 코드에서 살펴보면 다음과 같은 형태로 되어 있습니다.
``` java
public ComponentName resolveActivity(@NonNull PackageManager pm) {
    if (mComponent != null) {
        return mComponent;
    }

    ResolveInfo info = pm.resolveActivity(
        this, PackageManager.MATCH_DEFAULT_ONLY);
    if (info != null) {
        return new ComponentName(
                info.activityInfo.applicationInfo.packageName,
                info.activityInfo.name);
    }

    return null;
}
```

즉 가장 적당한 intent를 골라주는데 사용할 수 있습니다. startActivity에서도 내부에 resolveActivity가 구현되어 있습니다.
이러한 로직을 이용하여 널 스트링이 반환되면 적절한 intent가 없다는 것을 알 수 있는 것입니다.

## queryIntentActivities()

그렇다면 queryIntentActivities는 무엇일까요? 이름에서도 알 수 있듯이 가능한 액티비티를 모두 반환해줍니다.

반환값에 대한 개발자 문서를 살펴보면
>Returns a List of ResolveInfo objects containing one entry for each matching activity, ordered from best to worst. In other words, the first item is what would be returned by resolveActivity(Intent, int). If there are no matching activities, an empty list is returned. This value cannot be null.

우선순위가 높은 것부터 낮은 순서대로 리스트 형태로 반환하는 것을 알 수 있습니다. 또한 첫번째 아이템이 resolveActivity의 리턴값임을 확인할 수 있습니다.

## Reference
<https://developer.android.com/reference/android/content/Intent?hl=ko#resolveActivity(android.content.pm.PackageManager)><br>
<https://developer.android.com/reference/android/content/pm/PackageManager#resolveActivity(android.content.Intent,%20int)><br>
<https://developer.android.com/reference/android/content/pm/PackageManager#queryIntentActivities(android.content.Intent,%20int)>