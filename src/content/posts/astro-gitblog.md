---
title: Astro 블로그 시작하기
author: Zo0ls
pubDatetime: 2026-11-02T00:00:00Z
modDatetime:
slug: astro-github-pages
featured: false
draft: false
tags:
  - Astro
  - GitHub
  - Blog
description: Astro 테마 선택부터 GitHub 저장소 생성, GitHub Actions 설정, 자동 배포까지 정리한 가이드.
---

Astro를 이용하면 가볍고 빠른 정적 블로그를 만들 수 있다. 다양한 무료 테마를 활용하면 디자인을 처음부터 만들 필요도 없다.

이 글에서는 Astro 테마를 가져와 로컬에서 실행하고, GitHub Pages를 통해 배포하는 과정을 정리한다.

## Astro를 선택한 이유

처음에는 단순히 개인적인 기록을 남길 수 있는 블로그가 필요했다.

복잡한 기능이나 별도의 서버 관리보다는 글을 작성하고, 필요할 때 수정할 수 있는 가벼운 환경을 원했다.

여러 블로그 플랫폼과 정적 사이트 생성기를 살펴보던 중 Astro를 알게 되었다.

Astro를 선택한 이유는 다음과 같다.

- **가벼운 구조:** 불필요한 JavaScript를 최소화해 빠른 페이지 로딩이 가능하다.
- **Markdown 지원:** 별도의 에디터 없이 Markdown 파일만으로 글을 작성할 수 있다.
- **다양한 테마:** 처음부터 디자인을 만들지 않아도 기존 테마를 활용할 수 있다.
- **정적 사이트 생성:** 서버를 별도로 운영하지 않고 GitHub Pages에 배포할 수 있다.
- **GitHub 연동:** 글을 작성하고 push하면 자동으로 빌드와 배포가 진행된다.

무엇보다 Markdown 파일만으로 글을 관리할 수 있다는 점이 마음에 들었다.

복잡한 시스템을 관리하기보다는 기록 자체에 집중할 수 있는 환경이라는 점에서 Astro가 적합하다고 생각했다.

## 1. GitHub 저장소 생성

먼저 GitHub에서 새로운 Repository를 생성한다.

개인 블로그를 운영할 목적이라면 저장소 이름을 다음과 같이 설정한다.

```text
사용자명.github.io
```

예를 들어 GitHub 사용자명이 Zo0ls라면 다음과 같다.
text

Zo0ls.github.io

저장소 생성 시 다음과 같이 설정한다.

    Repository name: 사용자명.github.io

    Visibility: Public

    Add a README file: 체크 해제

    Add .gitignore: None

    Choose a license: None

이렇게 생성하면 다음 주소로 블로그에 접속할 수 있다.
```text
https://사용자명.github.io/
```
## 2. Astro 테마 선택

Astro는 다양한 블로그 테마를 제공한다.

대표적인 테마는 다음과 같다.

    AstroPaper 

    Astro 공식 테마 목록 

원하는 테마의 GitHub 저장소에서 프로젝트를 가져온다.

이 글에서는 AstroPaper를 기준으로 진행한다.

## 3. 로컬 환경에서 실행

프로젝트를 가져왔다면 터미널에서 프로젝트 폴더로 이동한다.
```bash
cd 프로젝트폴더
```
의존성을 설치한다.
```bash
npm install
```
개발 서버를 실행한다.
```bash
npm run dev
```
브라우저에서 다음 주소로 접속한다.
```text
http://localhost:4321/
```
정상적으로 사이트가 표시되는지 확인한다.
## 4. 사이트 주소 설정

astro.config.ts 또는 astro.config.mjs에서 사이트 주소를 설정한다.
```ts

import { defineConfig } from "astro/config";

export default defineConfig({
  site: "https://사용자명.github.io",
});
```
개인 도메인 저장소(사용자명.github.io)를 사용하는 경우 일반적으로 base 설정은 필요하지 않다.

일반 프로젝트 저장소를 사용하는 경우에는 저장소 경로에 맞는 base 설정이 필요하다.
## 5. GitHub Pages 설정

이 단계는 반드시 확인해야 한다.

GitHub 저장소에서 다음 경로로 이동한다.

Settings → Pages → Build and deployment

Source 항목을 다음과 같이 변경한다.
```text

GitHub Actions
```
Astro는 자체적으로 빌드한 정적 파일을 배포해야 하므로 GitHub Actions를 사용한다.

기본 설정인 Jekyll 방식으로 배포하면 Astro 프로젝트가 정상적으로 처리되지 않을 수 있다.
## 6. GitHub Actions 배포 설정

프로젝트 루트에 다음 디렉터리와 파일을 생성한다.
```text

.github/
└── workflows/
    └── deploy.yml
```
deploy.yml에 다음 내용을 작성한다.
```yaml

name: Deploy Astro to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - uses: withastro/action@v6

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy
        id: deployment
        uses: actions/deploy-pages@v5
```
이 설정은 GitHub에 코드를 push할 때 자동으로 Astro 프로젝트를 빌드하고 GitHub Pages에 배포한다.

참고: Astro 공식 GitHub Pages 배포 가이드 
7. GitHub에 프로젝트 업로드

이제 로컬 프로젝트를 GitHub 저장소에 업로드한다.

아직 Git 저장소를 초기화하지 않았다면 다음 명령어를 실행한다.
```bash

git init
git branch -M main
git add .
git commit -m "Initial commit"
```
원격 저장소를 연결한다.
```bash

git remote add origin https://github.com/사용자명/사용자명.github.io.git
```
GitHub에 업로드한다.
```bash

git push -u origin main
```
이미 Git 저장소가 초기화되어 있거나 원격 저장소가 연결되어 있다면 중복으로 실행하지 않는다.
8. 배포 확인

GitHub 저장소의 Actions 탭으로 이동한다.

배포 과정은 다음과 같다.

    GitHub에 코드 업로드

    Astro 프로젝트 빌드

    정적 파일 생성 (dist/)

    GitHub Pages에 배포

배포 작업이 성공하면 초록색 체크 표시가 나타난다.

이후 다음 주소에서 블로그를 확인할 수 있다.
```text

https://사용자명.github.io/
```
9. 이후 블로그 관리

초기 배포가 완료되면 이후에는 글을 작성하고 GitHub에 업로드하기만 하면 된다.
```bash

git add .
git commit -m "Add new post"
git push
```
GitHub Actions가 자동으로 빌드와 배포를 진행한다.

별도로 서버를 관리하거나 수동으로 파일을 업로드할 필요가 없다.
Conclusion

Astro를 이용하면 복잡한 서버 설정 없이도 개인 블로그를 운영할 수 있다.

처음 시작할 때 중요한 것은 다음 세 가지다.

    원하는 Astro 테마 선택

    astro.config에서 사이트 주소 설정

    GitHub Pages를 GitHub Actions 방식으로 배포

이 세 가지를 제대로 설정하면 이후에는 글 작성과 관리에만 집중할 수 있다.





