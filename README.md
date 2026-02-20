# Lotimuah's Blog

기술 블로그 - Beautiful Jekyll 테마 사용

## 로컬에서 실행

```bash
bundle install
bundle exec jekyll serve
```

브라우저에서 http://localhost:4000 접속

## 배포

GitHub에 push하면 자동으로 GitHub Pages에 배포됩니다.

## 포스트 작성

`_posts/` 폴더에 `YYYY-MM-DD-제목.md` 형식으로 작성하세요.

```yaml
---
layout: post
title: "포스트 제목"
tags: [Docker, Linux]
---
```
