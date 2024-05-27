# zzingyuna.github.io



## 로컬에서 Jekyll Theme 확인하는 방법

필수설치 언어: ![Ruby](https://img.shields.io/badge/ruby-%23CC342D.svg?style=for-the-badge&logo=ruby&logoColor=white)

cmd에서 아래 명령문 실행
```
gem install jekyll bundler
bundle install
gem install tzinfo
bundle lock --add-platform x86_64-linux
```

Gemfile에 아래 내용 추가
```
gem 'tzinfo'
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw]
```

cmd에서 아래 명령문 실행
```
bundle exec jekyll serve
```



## github 사이트 배포 시 에러 해결 사항
- 'bundle lock --add-platform x86_64-linux' 명령어 실행
- Gemfile.lock, _sass/dist, assets/js/dist 파일 .gitignore에서 제외
- 'npm install -g win-node-env' 명령어 실행
- 'npm install && npm run build' 명령어로 assets/js/dist 폴더 생성



## Chirpy Jekyll Theme Features

- Dark Theme
- Localized UI language
- Pinned Posts on Home Page
- Hierarchical Categories
- Trending Tags
- Table of Contents
- Last Modified Date
- Syntax Highlighting
- Mathematical Expressions
- Mermaid Diagrams & Flowcharts
- Dark Mode Images
- Embed Media
- Comment Systems
- Built-in Search
- Atom Feeds
- PWA
- Web Analytics
- SEO & Performance Optimization

## Documentation

To learn how to use, develop, and upgrade the project, please refer to the [Wiki][wiki].



[gem]: https://rubygems.org/gems/jekyll-theme-chirpy
[ci]: https://github.com/cotes2020/jekyll-theme-chirpy/actions/workflows/ci.yml?query=event%3Apush+branch%3Amaster
[codacy]: https://app.codacy.com/gh/cotes2020/jekyll-theme-chirpy/dashboard?utm_source=gh&utm_medium=referral&utm_content=&utm_campaign=Badge_grade
[license]: https://github.com/cotes2020/jekyll-theme-chirpy/blob/master/LICENSE
[jekyllrb]: https://jekyllrb.com/
[clipartmax]: https://www.clipartmax.com/middle/m2i8b1m2K9Z5m2K9_ant-clipart-childrens-ant-cute/
[demo]: https://cotes2020.github.io/chirpy-demo/
[wiki]: https://github.com/cotes2020/jekyll-theme-chirpy/wiki
[contribute-guide]: https://github.com/cotes2020/jekyll-theme-chirpy/blob/master/docs/CONTRIBUTING.md
[contributors]: https://github.com/cotes2020/jekyll-theme-chirpy/graphs/contributors
[lib]: https://github.com/cotes2020/chirpy-static-assets
[vscode]: https://code.visualstudio.com/
[jetbrains]: https://www.jetbrains.com/?from=jekyll-theme-chirpy
