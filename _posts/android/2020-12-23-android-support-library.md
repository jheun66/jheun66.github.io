---
layout: post
title: ASL과 AndroidX Library
date: 2020-12-24 6:08:00 +0900
categories: [Android]
tags: [Android, AndroidX, ASL]
---

안드로이드 관련된 예전 프로젝트를 살펴보면 android.support.v4, v7 같은 라이브러리가 코드내에 추가되어 있을 것입니다.
이러한 안드로이드 지원 라이브러리(ASL) 패키지는 안드로이드 프레임워크의 API에 포함되어 있지 않습니다. 주로 구 버전의 안드로이드 플랫폼에서 지원되지 않는 기능을 지원하기 위해 사용됩니다. 밑의 참조 링크를 통해 문서에 접근하면 Deprecated된 문서임을 알 수 있습니다.

> 공식 개발자 문서 참고 : Android 9.0(API 수준 28)의 출시와 함께 Jetpack의 일부인 새로운 버전의 지원 라이브러리 AndroidX가 제공됩니다. AndroidX 라이브러리는 기존 지원 라이브러리를 포함하며 최신 Jetpack 구성요소 또한 포함합니다.
> 
> 지원 라이브러리를 계속 사용할 수 있습니다. 이전 아티팩트(버전 27 이상이며 android.support.*로 패키징됨)는 Google Maven에서 계속 사용할 수 있습니다. 그러나 모든 신규 라이브러리 개발은 AndroidX 라이브러리에서 진행됩니다.
> 
> 모든 신규 프로젝트에서 AndroidX 라이브러리를 사용하는 것이 좋습니다. 기존 프로젝트를 AndroidX로 이전하는 것 또한 고려해 보세요.

## 지원 라이브러리와 패키지 이름
v# 표기법은 원래 지원하는 최소 API 레벨을 나타내고 있었지만 지원 라이브러리 버전 26.0.0 (2017년 7월 버전)부터 모든 지원 라이브러리 패키지 대상으로 지원되는 최소 API 레벨이 Android 4.0(API 레벨 14)으로 변경되었습니다. 이러한 이유로 지원 라이브러리의 최신 버전을 사용할 때 v# 패키지 표시법이 최소 API 지원 레벨을 나타낸다고 볼 수 없습니다. 최신 버전의 이러한 변화는 v4와 v7 라이브러리 패키지가 동일한 최소 API 수준을 지원한다는 것을 의미합니다.

버전 기록을 통해 2018년 9월 21일에 version 28.0.0 이후로 support 패키지는 업데이트 하지않는 것을 알 수 있습니다. androidx로 migration을 추천하고 있습니다.  

## AndroidX
지원 라이브러리는 종류가 많고 서로 다른 네임스페이스를 가지고 있어 유지 관리가 힘들었습니다. 
이를 해결하고자 Android Jetpack 라이브러리에 `androidx` 라는 일관된 네임스페이스를 사용하여 관리하도록 하였습니다. 기존 지원 라이브러리 패키지는 상응하는 `androidx.*`패키지에 매핑되어 있습니다.

Jetpack은 개발자가 관심 있는 코드에 집중할 수 있도록 권장사항 준수, 상용구 코드 제거, 모든 Android 버전과 기기에서 일관되게 작동하는 코드 작성을 돕는 라이브러리 모음입니다.



## Reference
[**지원 라이브러리**](https://developer.android.com/topic/libraries/support-library?hl=ko#framework-apis)<br/>
[**AndroidX 개요**](https://developer.android.com/jetpack/androidx?hl=ko)
