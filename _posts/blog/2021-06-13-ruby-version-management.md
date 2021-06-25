---
layout: post
title:  "Ruby Version Management"
date:   2021-06-13 21:01:11 +0900
categories: [Blog]
tags: [ruby, rbenv, github pages]
---
github pages를 만드는 과정에서 다른 루비 버전을 설치하고 관리하다 꼬인거 같아서 rbenv를 사용해서 관리하기로 했습니다.

### 이전에 설치한 루비 버전 삭제

```bash
brew uninstall ruby
```

### rbenv 설치

```bash
brew install rbenv ruby-build
```

### bash_profile 수정

`~/.bash_profile`에 `eval "$(rbenv init -)"` 추가한 후

```bash
source ~/.bash_profile
```

### 루비 버전 설치

설치할 수 있는 루비 버전 확인 후

```bash
rbenv install -l
```

설치할 수 있는 버전 설치 

```bash
rbenv install 3.0.1
```

### 버전 적용

설치 되어있는 버전 확인

```bash
rbenv versions
```

글로벌 버전 세팅

```bash
rbenv global 3.0.1
```

로컬 버전 세팅

```bash
rbenv local 3.0.1
```

적용된 버전 확인

```bash
rbenv version
```

적용된 루비 버전 확인

```bash
ruby -v
```

## Reference
[stackoverflow](https://stackoverflow.com/questions/61196279/how-to-remove-all-old-ruby-versions-and-version-managers-and-reinstall-a-singl)