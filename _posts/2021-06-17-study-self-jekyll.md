---
layout: post
title:  "[Jekyll] 블로그 개발 일지"
subtitle:   "혼자 배워보는 Jekyll"
categories: study
tags: self
comments: true
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
  * css가 맘에 든다!



## 오류

### 가장 큰 오류

* zip을 다운로드 받아 내 폴더로 가져오면서 경로가 바뀌어서 그런지 여러 오류가 생긴다.
  * Tag를 인식 못한다거나, main.scss를 compile하지 못하는 오류

=> 처음부터 만들어보고 싶었지만, 어쩔 수 없이 되는걸 가져와서 진행하기로 결정... (210610)



## 수정한 것

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



### 구글 검색엔진 노출(잘 안돼서 진행중)

> [참고1](https://mkl0819.github.io/category/blog/2019-12-07-google-search/#_navigation)
>
> [참고2](https://maejinkim.github.io/%EB%B8%94%EB%A1%9C%EA%B7%B8/%EA%B2%80%EC%83%89%EC%B6%94%EA%B0%80/)

1. [링크](https://search.google.com/search-console/welcome)로 들어가 내 깃허브 주소(`https://KiHoonAhn1.github.io`) 등록 후 `계속` 버튼 클릭
2. html 파일을 다운로드 받아 블로그 폴더의 `root` 위치로 이동
3. gem install
   * gem install jekyll-feed => plugins에 추가
   * gem install jekyll sitemap => plugins에 추가
4. `sitemap.xml `파일 생성 후 코드 작성
5. `feed.xml` 파일 생성 후 코드 작성
6. `robots.txt` 파일 생성 후 코드 작성
   * 검색엔진 크롤링하는 것을 도와준다
7. `_config.yml`의 url 확인
8. 블로그 업데이트 후, `확인` 버튼 클릭
9. left-navbar에서 Sitemaps에 sitemap.xml 제출
10. 며칠 지나면 **가져올 수 없음**에서 정상으로 바뀐다고 한다.



### nav 수정

> overflow 기능이 있긴 하지만, 없는 것이 한눈에 보기 좋을 것 같아 간격 조정
>
> 기존 template은 br로 간격 조정을 해둔 부분이 많았다.

* `includes/sidebar.html`
  * br 몇개 제거
* `hydejack/_sidebar-inline.scss`
  * `.sidebar-sticky > h1`만들어서 margin 속성 추가
    * 기본적인 h1 style이 있기 때문에 따로 추가해줘야 한다.
  * left와 right 위치 변경
* `assets/tipuesearch/css/tipuesearch.css`
  * tipue\_search\_icon의 width, height 변경
  * left와 right의 border 색 변경
  * left의 max-height을 40px로 변경
* `assets/main_left.jpg`
  * 왼쪽 배경사진 변경
* img 상위에 button 추가 후, `tipue_search_button` class로 주고 속성 추가
  * 이미지 클릭으로도 검색 가능하게 만듦



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

* ~~구글에 내 게시물 노출시키기~~
* 광고
* Book - 좋은 문장 page layout 바꾸기
  * 작가명 노출을 위해 한 줄 정도만 보여주고, 'Continue reading!' 부분을 지우고 싶다
  * `_includes/page.html` 에서 바꿔줘야 함
* 노출된 태그의 횟수 별로 정리하는 기능



## 깨달은 것

* `<font color = blue> 글자 </font>` 로 글자 색 변경
