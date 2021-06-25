---
layout: post
title: Authentication VS Authorization
date: 2020-12-13 15:00:00 +0900
categories: [Security]
tags: [Authentication, Authorization, Word Meaning]
---

OAuth 2.0을 공부하면서 먼저 정리해야할 용어라 생각되어 작성합니다.

## Authentication
인증, 사용자가 자기 자신을 <u>증명</u>하는 것


## Authorization
인가, 권한 부여, 사용자가 리소스에 접근할 <u>권한</u>을 주는 것


## Authentication VS Authorization
비슷한 용어처럼 들리지만 다른 과정이라는 것을 알 수 있습니다.

| Authentication   | Authorization     |
|:-----------------|:------------------|
| 신원 증명 | 권한 승인 혹은 거절 |
| 비밀 번호나 생체 인식 등 유효한 자격을 가지고 있는지 확인 | 정책과 규칙을 통해 접근 가능 여부 결정 |
| 사용자에게 보임 | 사용자에게 보이지 않음 |
| OpenID Connect protocol | OAuth 2.0 framework |


## Reference
<https://www.okta.com/identity-101/authentication-vs-authorization/><br/>
<https://auth0.com/docs/authorization/authentication-and-authorization>