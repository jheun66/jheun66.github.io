---
layout: post
title: ACTION_SEND & ACTION_SENDTO(이메일 인텐트)
date: 2021-01-14 11:30:00 +0900
categories: [Android]
tags: [Activity, Intent]
---

이메일 보내기 인텐트를 사용할 때 참고할 내용들을 정리합니다.

## ACTION_SEND
``` java
private void sendEmailwithSend() {
    Intent intent = new Intent(Intent.ACTION_SEND);
    intent.setType("plain/text");
    intent.putExtra(Intent.EXTRA_EMAIL, new String[] { "jheun5@naver.com" , "someone.example.com"});
    intent.putExtra(Intent.EXTRA_SUBJECT, "이메일 테스트 제목");
    intent.putExtra(Intent.EXTRA_TEXT, "이메일 테스트 내용1:\n이메일 테스트 내용2:");
    if (intent.resolveActivity(getPackageManager()) != null) {
        startActivity(intent);
    }
}
```
EXTRA를 통해 내요을 추가할 수 있습니다. EXTRA_CC, EXTRA_BCC 도 물론 추가할 수 있습니다.
EXTRA_STREAM을 사용할 때는 타입을 */* 로 합니다.
좀더 자세한 사용법은 참고의 개발자 문서를 확인하면 되겠습니다.

![select send](/assets/img/android_example/selectSend.jpg)
대신 선택창에 SEND를 받는 앱이 많습니다.

## ACTION_SENDTO
``` java
private void sendEmailwithSendTo() {
    Intent intent = new Intent(Intent.ACTION_SENDTO);
    String emailTitle = "이메일 테스트 제목";
    String emailContent = "이메일 테스트 내용1:\n이메일 테스트 내용2:";

    String uriText =
            "mailto:" + "jheun5@naver.com" +
                    "?subject=" + Uri.encode(emailTitle) +
                    "&body=" + Uri.encode(emailContent);

    Uri uri = Uri.parse(uriText);
    intent.setData(uri);
    if (intent.resolveActivity(getPackageManager()) != null) {
        startActivity(intent);
    }
}
```
ACTION_SENDTO의 경우에는 DATA URI만 받습니다. 이때 mailto Scheme을 사용하여 내용을 채워서 보낼 수 있습니다.
그래서 이 인텐트는 EXTRA_STREAM을 받지 못합니다. 
[mailto 관련](https://css-tricks.com/snippets/html/mailto-links/)

![select sendto](/assets/img/android_example/selectSendTo.jpg)
이 경우 메일 앱만 받습니다.

## 좀 더 좋은 방법
``` java
private void sendEmail() {
    Intent emailSelectorIntent = new Intent(Intent.ACTION_SENDTO);
    emailSelectorIntent.setData(Uri.parse("mailto:"));

    Intent intent = new Intent(Intent.ACTION_SEND);
    intent.putExtra(Intent.EXTRA_EMAIL, new String[]{ "jheun5@naver.com" });
    intent.putExtra(Intent.EXTRA_SUBJECT, "이메일 테스트 제목");
    intent.putExtra(Intent.EXTRA_TEXT, "이메일 테스트 내용1:\n이메일 테스트 내용2:");

    intent.setSelector( emailSelectorIntent );

    // 첨부파일 관련
    //intent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
    //intent.addFlags(Intent.FLAG_GRANT_WRITE_URI_PERMISSION);
    //Uri attachment = FileProvider.getUriForFile(this, "my_fileprovider", myFile);
    //intent.putExtra(Intent.EXTRA_STREAM, attachment);

    if( intent.resolveActivity(getPackageManager()) != null )
        startActivity(intent);
}
```

따라서 첨부 파일을 포함할 수 있게하고 이메일 앱만 실행시키고 싶을 때는 다음과 같이 셀렉터는 ACTION_SENDTO를 사용하고 ACTION_SEND로 보내면 됩니다.

첨부파일 관련해서는 다른 예제를 통해 정리하겠습니다.

## Reference
<https://developer.android.com/reference/android/content/Intent?hl=ko#ACTION_SEND><br>
<https://github.com/codepath/android_guides/wiki/Common-Implicit-Intents><br>
<https://stackoverflow.com/questions/4883199/using-android-intent-action-send-for-sending-email><br>


