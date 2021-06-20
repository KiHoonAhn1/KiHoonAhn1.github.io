---
layout: post
title:  "[Jekyll] 블로그 개발 일지"
subtitle:   "혼자 배워보는 Jekyll"
categories: study
tags: self
comments: true
header-img: img/review/2019-07-08-review-book-game-algorithm-1.jpg
---

> 혼자 만들어보는 git blog



# Jekyll

> 원래는 vue 기반으로 제작할까 고민했지만, 어차피 서버가 없는 Static site를 만들 거라면 다양한 기능을 제공하는 Jekyll이 더 적합하겠다 생각하였다.
>
> vue와 Jekyll을 함께 사용하는 방법을 찾아봤지만 정보가 별로 없었고, Jekyll에 대해 잘 알지 못해 알아보다 보니 Jekyll로만 블로그를 만들어보기로 결정했다.



## Blog 제작 과정

1. [Ruby 설치](https://rubyinstaller.org/downloads/)
   * `=>` 표시된 것으로 다운받는 것이 가장 안정적이다.

```bash
$ gem install jekyll
# update하라고 시킨다면
# 시간이 엄청나게 걸린다, 뭔가 문제가 있음(뭔 native 설정하는게 있음)
$ gem update
```



2. Jekyll 설치

> Start Command Prompt with Ruby 실행

```bash
# 내 git 폴더로 이동
$ cd C:\Users\kihoon\Desktop\SSAFY 수업 자료\KiHoonAhn1.github.io
# Jekyll 관련한 것들 다운로드
$ gem install bundler jekyll minima jekyll-feed tzinfo-data rdiscount
# 초기화 설정
$ jekyll new KiHoonAhn1.github.io
# 서버 구동
$ bundle exec jekyll serve
```

3. Theme 결정

* https://theorydb.github.io/
  * 다른 기본 테마로 하려니 오류가 너무 많이 나서 이분 깃허브를 다운로드 받아 수정함
* https://www.jihyeleee.com/
  * css가 맘에 든다. 가져오자!!



## 오류

### 가장 큰 오류

* zip을 다운로드 받아 내 폴더로 가져오면서 경로가 바뀌어서 그런지 여러 오류가 생긴다.
  * Tag를 인식 못한다거나, main.scss를 compile하지 못하는 오류

=> 처음부터 만들어보고 싶었지만, 어쩔 수 없이 되는걸 가져와서 진행하기로 결정... (210610)



## 수정 중

### _config.yml

* title
  * window에 보여질 이름
* url
  * 내 깃허브 url
* baseurl
  * http://127.0.0.1:4000/ 뒤에 들어갈 이름
  * 일단 `""`로 설정해두니 편하다
* tagline
  * 블로그 이름 밑에 나올 블로그 설명
* description
  * 메뉴바 사진 밑에 나올 내 설명
* author
  * name을 `min -> kihoon` 수정
  * email을 내 이메일로 수정
* disqus_shortname
  * 내 이름으로 수정
* copyright
  * 날짜와 이름 수정
  * footer에 보여질 이름



### Data 수정

* _data/authors.yml
  * 데이터를 나의 데이터로 수정 (수정한 부분이 굉장히 많음)



### categories 추가 / 수정

* `_featured_categories`에 추가하고 싶은 카테고리를 md파일로 만들고 다른 md 파일을 참고해 내용을 작성한다.
* `_featured_tags`에서 `카테고리 이름-보여질 이름` 으로 md 파일을 만들고, 마찬가지로 다른 md파일을 참고해 내용을 작성한다.

* **이후에 수정 부분도 해야지**



### about.md

* about 메뉴에 나오는 내 소개를 수정했다.
  * 나중에 기술 stack에 badge 달아서 넣고, 기타 등등 내 소개를 잘 넣어둬야겠다!



### _posts

* 기존 post 전부 삭제했음



#### 유의할 점 + 알아둘 것

* **md 파일 제목은 무조건 영문 ㅠㅠ**

* **`date`** in **`Category`** on **`tags`**
  * 소제목에 이렇게 나온다
  * tags가 뿌려지는 방식이 맘에 들지 않는다.
  * badge나 좀 둥그런 회색 배경에 하나씩 들어가있는 방식으로 보여주는게 더 이쁠 것 같다!
  
* tags에 들어가있는 것으로 분류가 되나?

* Django code 중 `block`이나 `extends` 같은 태그들이 'ruby tag'로 인식되어 오류가 생기므로 아래와 같이 `raw` 태그를 사용해줘야 한다.

  ```python
  {% raw %}
      {% block content %}
  
      {% endblock %}
  {% endraw %}



### category-list

| 필요한 목록 | 용도                              | 세부 목록                   |
| ----------- | --------------------------------- | --------------------------- |
| Study       | 공부한 것 정리하기                | class, 자습, TIL            |
| Algorithm   | 알고리즘                          |                             |
| Book        | 주로 책 리뷰, 좋았던 한마디, 기타 | Book Review, 맘에 드는 문장 |
| Project     | 프로젝트 정리해서 올리기          |                             |
| About       |                                   |                             |

* 책 한마디 같은 경우, 책 하나마다 미리보기 크기가 좀 작아졌으면 좋겠다.



### 댓글

* disqus
  * short_name 수정



## 고치고 싶은 것

* 현재 어디 있는지, left-navbar에 나타내주자
  * 좀 눈에 띄게 나타내주자. 어디 있는지 도통 모르겠다!
  * `_sass/hydejack/_sidebar-inline.scss` 에서 css 변경 !!! (찾는데 1시간이나 걸렸네...)
  * `_includes/nav.html`에서 관련 class 변경!!
    * active는 작동을 하나, **실시간으로** 변경되질 않는다.... rendering을 다시 해야 그때 class가 변경된다....
* 첫 페이지를 About 페이지로!
  * 현재 첫 페이지는 `_includes/post.html`
  * `_includes/blog.html` 에서 바꿔줘야 하는데 아직 잘 모르겠다..
  * 아니면!! 카테고리와 연도별로 두번 나눠서 전체를 보여주는 것도 괜찮지 않을까?
* Project Category 생성
  * 사진 한 장 미리보기 기능을 넣자
    * `_includes/post-list-item.html`을 수정하거나 프로젝트용 list를 생성해주자
