# 🌈 Jack koh 기술 블로그

안녕하세요. 프론트엔드개발자로 재직중인 고현영입니다.

제 블로그는 [GitHub Pages](https://pages.github.com/)와 [Jekyll](https://jekyllrb.com/)을 사용하여 퍼블리싱하였습니다.  
해당 문서는 Jekyll 기반의 블로그 운영을 위한 환경구성 방법과 설치방법들을 설명합니다.

jack-koh blog: https://github.com/Jack-koh/Jack-koh.github.io.git
jekyll 공식사이트: https://jekyllrb-ko.github.io/


## Installation

1️⃣. 프로젝트 clone하기
```sh
$ git clone https://github.com/Jack-koh/Jack-koh.github.io.git
$ cd Jack-koh.github.io
```

2️⃣. 환경 구성하기

| Name | README |
| ------ | ------ |
| Git | https://git-scm.com/ |
| Ruby | https://www.ruby-lang.org/ko/ |
| Bundler | https://bundler.io/ |
| VSCode | https://code.visualstudio.com/download |

로컬환경에서 사이트를 실행하려면 Ruby 와 Bundler가 필요합니다.
설치후 제대로 설치가 되었는지 확인합니다.

`루비 설치 확인`
```sh
$ ruby -v
$ ruby 3.0.3p157 (2021-11-24 revision 3fb7d2cadc) [x64-mingw32]
```

`bundler 설치후 확인`
```sh
$ gem install bundler
$ bundler -v
Bundler version 2.2.33
```

`아래의 명령어는 Jekyll 을 설치합니다.`
```sh
$ bundle update
$ bundle add jekyll
$ bundle update
$ bundle install
```

`로컬 환경에서 blog 실행하기`
```sh
$ bundle exec jekyll serve --watch
```

실행후 http://localhost:4000 접속 확인
