# zzingyuna.github.io



## 로컬에서 Jekyll Theme 확인하는 방법

필수설치 언어: ![Ruby](https://img.shields.io/badge/ruby-%23CC342D.svg?style=for-the-badge&logo=ruby&logoColor=white)


#### cmd에서 아래 명령문 실행
```
gem install jekyll bundler
bundle exec jekyll serve
```

[http://localhost:4000/](http://localhost:4000/) 사이트 확인



##  로컬에서 Jekyll Theme 확인 시 에러 해결 사항
#### cmd에서 명령어 실행
```
gem install tzinfo
```

#### Gemfile에 아래 내용 추가
```
gem 'tzinfo'
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw]
```

