---
title: GitHub 블로그에 Jekyll Chirpy 테마 적용하기
description: Jekyll Chirpy 테마 적용 기록
author: yuna
date: 2024-07-23 20:58:00 +0900
categories: [Blogging]
tags: [Blogging]
pin: false
math: true
mermaid: true
---

## GitHub 소스 받기

먼저 자신의 GitHub 사이트에서 repositories 탭으로 접속합니다. 그리고 만들었던 GitHub 블로그(사용자ID.github.io) 레파지토리에 들어가서 소스를 클론 받습니다.  
![Desktop View](assets/img/postImages/2024-07-23-apply-chirpy-thema/1.png){: width="972" height="589" }
_Clone repositories_


그리고 적용할 테마 주소로 들어가서 소스를 다운 받습니다.  
![Desktop View](assets/img/postImages/2024-07-23-apply-chirpy-thema/2.png){: width="500" height="581" }
_Create a new repository_


## Chirpy 테마의 소스 적용

다운받은 테마 소스 중 아래 이미지와 같이 선택한 파일을 자신의 소스에 복붙해줍니다.  
![Desktop View](assets/img/postImages/2024-07-23-apply-chirpy-thema/3.png){: width="500" height="581" }
_Create new file_


## 로컬에서 웹을 실행해보기

먼저 [Ruby](https://www.ruby-lang.org/en/downloads/)로 접속해서 설치파일을 받고 설치를 진행해주세요.  

Ruby 설치 파일을 받는 동안 소스에서 `npm install` 명령어와 `npm run build` 명령어를 실행해주세요. 디자인이 로컬에서도 잘 적용되어 보이도록 하기 위해 클라이언트 소스 빌드를 진행합니다.  

설치 후 `ruby -v` 명령어를 실행하여 아래와 같이 버전이 나타나면 성공입니다.
![Desktop View](assets/img/postImages/2024-07-23-apply-chirpy-thema/4.png){: width="500" height="581" }
_Commit changes_

그리고 복사 붙여넣기 한 소스에 Gemfile을 열어 아래와 같이 문구를 추가해주세요.  
```text
gem 'tzinfo'
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw]
```
![Desktop View](assets/img/postImages/2024-07-23-apply-chirpy-thema/5.png){: width="500" height="581" }
_Commit changes_

소스에서 `gem install jekyll bundler` 명령어를 실행시켜 주세요. 아래와 같이 '6 gems installed'가 나오면 성공입니다.  
![Desktop View](assets/img/postImages/2024-07-23-apply-chirpy-thema/6.png){: width="500" height="581" }
_Create a new repository_

그리고 `bundle exec jekyll serve` 명령어를 실행시켜 주세요.  
여러 빨간 에러가 표시되지만 마지막 문구는 4000번 포트로 실행중이라고 표시됩니다.  
![Desktop View](assets/img/postImages/2024-07-23-apply-chirpy-thema/7.png){: width="500" height="581" }
_Create a new repository_

웹 브라우저를 열어서 'http://localhost:4000'이라고 쳐서 들어가면 아래와 같이 테마가 적용된 화면이 보여지면 성공입니다.  
![Desktop View](assets/img/postImages/2024-07-23-apply-chirpy-thema/8.png){: width="500" height="581" }
_Create a new repository_


## 원하는 내용으로 수정해주기

소스 중 `_config.yml` 파일과 `_data/authors.yml`, `_data/contact.yml` 등 원하는 데이터로 수정해주세요.  


## GitHub에 올려서 사이트 적용하기

소스를 모두 푸시해주세요.  
그럼 아래와 같이 GitHub  
![Desktop View](assets/img/postImages/2024-07-23-apply-chirpy-thema/9.png){: width="500" height="581" }
_Create a new repository_

![Desktop View](assets/img/postImages/2024-07-23-apply-chirpy-thema/9.png){: width="500" height="581" }
_Create a new repository_

![Desktop View](assets/img/postImages/2024-07-23-apply-chirpy-thema/9.png){: width="500" height="581" }
_Create a new repository_

