---
layout: post
title:  "Jekyll 설치 및 테스트"
date:   2021-06-13 21:46:00 +0900
categories: [Blog]
tags: [jekyll, bundler, github pages]
---
### jekyll, bundler gems 설치

```bash
gem install jekyll bundler
```

### jekyll site 생성

```bash
# 폴더로 생성
jekyll new my_blog.github.io
# 혹은 본인의 git repository에서 실행할 경우
jekyll new ./
```

테마는 자동으로 기본 테마 [minima](https://github.com/jekyll/minima)

### bundle 설치 및 테스트

```bash
bundle install
bundle exec jekyll serve
```

[http://localhost:4000](http://localhost:4000/)에서 결과를 확인 가능

### 오류 상황일 경우

- `require': cannot load such file -- webrick

```bash
bundle add webrick
```

- `bind': address already in use - bind(2) for 127.0.0.1:4000 (errno::eaddrinuse)

```bash
lsof -wni tcp:4000
```

PID 체크 후 해당 프로세스 킬

```bash
kill -9 해당 PID
```

## Reference

[jekyll tutorial](https://jekyllrb.com/docs/step-by-step/01-setup/)

[webrick 오류](https://junho85.pe.kr/1850)

[로컬 호스트를 이미 사용하고 있는 경우](https://stackoverflow.com/questions/31039998/rails-address-already-in-use-bind2-errnoeaddrinuse)