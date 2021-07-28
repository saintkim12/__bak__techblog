---
title: 비공개 repository npm으로 배포하기
description:
date: 2021-07-28
tags:
  - npm
layout: layouts/post.njk
---

# 요약/혹은 더 보기 좋은 원본 글
[사내 유틸리티 메소드를 NPM 패키지로 관리하기](https://medium.com/@changjoopark/사내-유틸리티-메소드를-npm-패키지로-관리하기-e1741614611b)

# 재현
## 1. 공통 모듈 등 관리할 모듈을 git 저장소로 따로 빼기

## 2. git 주소 받기
그냥 클론할 때 사용하는 주소 받으면 됨

## 3. package.json에 정보 설정하여, 접근할 수 있는지 테스트
```js
// package.json
    "dependencies": {
        // ...
        "[module-name]": "git+http://[id]:[password]@[git-url]"
    }
```

## 4. 비밀번호 입력 대신 토큰 사용하기
### Access Token 만들기
- bitbucket(v6.8.1)은 Manage Account > Personal access tokens
- 토큰명 지정하고 생성(권한은 읽기 권한만 있으면 될듯)
    ![Tokens in bitbucket(v6.8.1)](/techblog/img/2021-07-28-13-03-29.png)
- 토큰 나오면 복사해두기
- package.json에 password 대신 토큰 입력하기
    ```js
    // package.json
      "dependencies": {
        // ...
        "[module-name]": "git+http://[id]:[access-token]@[git-url]"
      }
    ```

## 참고
[npm package.json 관련](https://docs.npmjs.com/files/package.json#git-urls-as-dependencies)
