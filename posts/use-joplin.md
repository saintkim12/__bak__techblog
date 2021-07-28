---
title: Joplin 사용하기
description: Note 도구인 Joplin을 사용해봄
date: 2021-07-28
tags:
  - note
  - joplin
layout: layouts/post.njk
---

# [Joplin](https://joplinapp.org/)
- 동기화 가능한 노트 프로그램


## 사용해본 이유(feat. Onenote)
### Onenote를 사용하는데 문제가 있음
- 주로 한 환경에서만 메모용으로 쓰는데,
  - 작성한 내용에 페이지 충돌 감지가 뜨고,(해결하려면 또 PC버전이었나 웹이었나로 하래서 무시함)
  - 급하게 적은 메모가 나중에 보니 아예 비어있었던,
    (급한 상황이라 조작미스거나 저장 자체가 안되었거나 그럴수도 있지만)
- 이런 경험이 있어 이대로는 못쓰겠다고 판단함

### 그래서 내 요구사항은,
- 필요최소한의 기능만 있어서 가벼워야 함
  - Mobile + PC Application
  - 동기화
  - 이미지 업로드
  - +@
    - markdown 지원
    -

## 특징
- WebDAV server를 구할 수 있다면 사용은 무료
  - 난 Dropbox에 붙임(2기가 주길래..)
- 텍스트 기반의 메모하기 좋음
  - markdown까지 알면 사용성 증대
- 터미널 버전이 있음
  - 터미널 버전에 자신이 좋아하는 에디터(난 vscode) 같은거 붙이면 익숙한 환경에서 작성 가능

## 아쉬웠던거?
- 이미지 업로드 시 리사이징 안되는듯?(확인은 안해봤음)
- 웹 버전 미지원(정확히 말하면 웹 호스팅 버전..)
  - 비공식? 프로젝트 자체는 있는거같은데 셀프 호스팅 해야함
    - [Joplin Web](https://discourse.joplinapp.org/t/joplin-web-web-application-companion-for-joplin/555)
    - [https://gitlab.com/annyong/joplin-web](https://gitlab.com/annyong/joplin-web)
    - [없는 이유](https://github.com/laurent22/joplin/issues/852#issuecomment-430136462) - 요약하면 web browser에는 SQLite가 없으니까
- markdown을 잘 사용하지 못할 경우 글 꾸미기같은거 하려면 좀 힘듬
  - markdown 자체가 그런게 쉽진 않다만..
