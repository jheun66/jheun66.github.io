---
layout: post
title: Activity Lifecycle
date: 2020-12-28 12:00:00 +0900
categories: [Android]
tags: [Activity]
mermaid: true
---

# Acitivity Lifecycle
여러 블로그나 서적마다 용어나 표현 스타일이 조금씩 다르지만 큰 플로우는 동일하였습니다. 플로차트는 안드로이드 개발자 문서 및 코드랩에서 가져왔습니다.

## 액티비티의 State와 Lifecycle callback
액티비티 인스턴스는 생명주기동안 몇가지 상태로 전환될 수 있습니다.  
![액티비티 상태변화](https://developer.android.com/codelabs/android-training-activity-lifecycle-and-state/img/6196fc9ecbe17d54.png)

![액티비티 생명주기](https://developer.android.com/guide/components/images/activity_lifecycle.png)

코드는 실습 포스트에 담도록 하겠습니다.

### onCreate
반드시 구현되어야하는 콜백입니다. 시스템이 액티비티를 처음 만들때 호출됩니다. 액티비티 초기화 및 UI를 인플레이팅합니다. 

### onStart
액티비티가 화면에 나타나면서 상호 작용할 준비를 합니다.

### onResume
_resumed_ 상태로 바뀌고 액티비티가 foreground에 나옵니다. 액티비티가 포커스를 가지고 사용자와 상호작용을 할 수 있습니다. _paused_ 에서 호출된 경우에는 해제한 리소스를 다시 할당해 주는 작업을 하면 됩니다(최대한 간단한 작업?). UI 상태 등은 onSaveInstanceState()와 onRestoreInstanceState(), onCraete()에서 수행해주도록 합니다. 

### onPause
포커스를 잃고 액티비티가 background로 가게됩니다. 일부 이벤트가 호출되거나 dialogbox가 화면에 나오거나 하면 화면이 일부 보이지만 포커스를 잃은 상태가 됩니다. 배터리 수명이나 메모리를 많이 차지하는 리소스들을 해제할 수 있습니다. 멀티윈도우의 경우도 화면은 보여도 포커스가 없는 상태가 됩니다. 문서에 따르면 멀티윈도우 같은 경우 UI 관련 리소스를 완전히 해제하거나 조정할 때는 onStop에서 처리하도록 권장하고 있습니다. 최대한 간단한 작업만 수행합니다. 

### onStop
_stopped_ 상태가 되면 더이상 사용자에게 화면이 보이지 않습니다. 새로 시작된 액티비티가 화면 전체를 차지할 경우나 화면회전등의 경우가 있습니다. cpu 부하가 큰 작업을 종료시켜야합니다. 문서에서는 데이터베이스에 저장할 시기등을 예로 들었습니다. 정지됨 상태에서 액티비티를 재개할 경우 onRestart()를 호출하고 액티비티가 종료되면 onDestroy()를 호출합니다. 추가적으로 ART가 프로세스를 종료 시킬 경우 onCreate() 부터 다시 호출되게 됩니다.

### onDestroy
액티비티가 소멸되기 전에 호출됩니다.

* 사용자가 back 버튼을 누르거나 finish()가 호출되는 경우
* configuration 변경으로 일시적으로 소멸 시키는 경우

2가지 경우로 나눌 수 있는데 첫번째 경우는 액티비티가 다시 생성되지 않으므로 소멸되기전에 모든 데이터를 정리해야 합니다.

## onSaveInstanceState()와 onRestoreInstanceState()
_Instance State_ 는 시스템이 이전 상태를 복원하기위한 Bundle 인스턴스입니다. 

플로차트에는 나오지 않았지만 ART에 의해 프로세스가 종료되었을 때 _onSaveInstanceState()_ 를 호출시켜 UI 상태 변화등을 저장하고 _onRestoreInstanceState()_ 에서 복구할 수 있습니다.

멀티윈도우 전환, 화면 가로세로 전환, home 버튼을 눌렀을 때, 다른 액티비티가 포어그라운드에 나올 때, 시스템이 메모리를 필요로하여 중요도가 낮은 프로세스를 죽일 때 호출될 수 있습니다.

액티비티가 명시적으로 중단되거나 사용자가 back 버튼을 눌렀을 때는 기본적으로 호출되지 않습니다. 

## 액티비티의 수명
수명에 대한 용어 정리 입니다. 

* 전체 수명

onCreate가 호출된 순간부터 onDestroy가 호출되어 액티비티가 종료될 때까지입니다. onDestroy 호출 없이 프로세스가 종료될 수 있습니다.

* 가시 수명

onStart가 호출된 순간부터 onStop이 호출될 때까지 가시 수명 상태가 됩니다. 이 구간에서는 사용자에게 화면이 보이게 됩니다.

* 활성 수명

onResume부터 onPause 호출까지 활성 수명 상태가 됩니다. 이때 사용자의 입력을 받을 수 있습니다.

## Reference
[**Android Developer**](https://developer.android.com/guide/components/activities/activity-lifecycle)<br/>
[**Android Developer CodeLabs**](https://developer.android.com/codelabs/android-training-activity-lifecycle-and-state?hl=en&continue=https%3A%2F%2Fcodelabs.developers.google.com%2F%3Fcat%3Dandroid#0)<br/>
리토마이어, 이안 레이크, "프로페셔널 안드로이드 4판", 현호철, Jpub, p82~88 