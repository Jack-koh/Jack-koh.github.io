# ๐ Jack koh ๊ธฐ์  ๋ธ๋ก๊ทธ

์๋ํ์ธ์. ํ๋ก ํธ์๋๊ฐ๋ฐ์๋ก ์ฌ์ง์ค์ธ ๊ณ ํ์์๋๋ค.

์  ๋ธ๋ก๊ทธ๋ [GitHub Pages](https://pages.github.com/)์ [Jekyll](https://jekyllrb.com/)์ ์ฌ์ฉํ์ฌ ํผ๋ธ๋ฆฌ์ฑํ์์ต๋๋ค.  
ํด๋น ๋ฌธ์๋ Jekyll ๊ธฐ๋ฐ์ ๋ธ๋ก๊ทธ ์ด์์ ์ํ ํ๊ฒฝ๊ตฌ์ฑ ๋ฐฉ๋ฒ๊ณผ ์ค์น๋ฐฉ๋ฒ๋ค์ ์ค๋ชํฉ๋๋ค.

jack-koh blog: https://jack-koh.github.io/  
jekyll ๊ณต์์ฌ์ดํธ: https://jekyllrb-ko.github.io/

## Installation

1๏ธโฃ. ํ๋ก์ ํธ cloneํ๊ธฐ

```sh
$ git clone https://github.com/Jack-koh/Jack-koh.github.io.git
$ cd Jack-koh.github.io
```

2๏ธโฃ. ํ๊ฒฝ ๊ตฌ์ฑํ๊ธฐ

| Name    | README                                 |
| ------- | -------------------------------------- |
| Git     | https://git-scm.com/                   |
| Ruby    | https://www.ruby-lang.org/ko/          |
| Bundler | https://bundler.io/                    |
| VSCode  | https://code.visualstudio.com/download |

๋ก์ปฌํ๊ฒฝ์์ ์ฌ์ดํธ๋ฅผ ์คํํ๋ ค๋ฉด Ruby ์ Bundler๊ฐ ํ์ํฉ๋๋ค.
์ค์นํ ์ ๋๋ก ์ค์น๊ฐ ๋์๋์ง ํ์ธํฉ๋๋ค.

`๋ฃจ๋น ์ค์น ํ์ธ`

```sh
$ ruby -v
$ ruby 3.0.3p157 (2021-11-24 revision 3fb7d2cadc) [x64-mingw32]
```

`bundler ์ค์นํ ํ์ธ`

```sh
$ gem install bundler
$ bundler -v
Bundler version 2.2.33
```

`์๋์ ๋ช๋ น์ด๋ Jekyll ์ ์ค์นํฉ๋๋ค.`

```sh
$ bundle update
$ bundle add jekyll
$ bundle update
$ bundle install
```

`๋ก์ปฌ ํ๊ฒฝ์์ blog ์คํํ๊ธฐ`

```sh
$ bundle exec jekyll serve --watch
```

์คํํ http://localhost:4000 ์ ์ ํ์ธ
