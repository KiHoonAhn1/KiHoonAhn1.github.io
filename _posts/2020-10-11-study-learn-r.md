---
layout: post
title:  "[R] R 설치&설정 및 내용 요약"
subtitle:   "수업시간에 배운 R"
categories: study
tags: learn R
comments: true
header-img: ''
---

> 수업 시간에 배운 R 설치&설정 방법과 내용 정리



# R

## 1. 시작 전

### 1-1. 설치

* **R** [링크](https://www.r-project.org)
  * download R -> 0-cloud -> Download R for Windows -> base ->  previous release -> ~~3.5.1~~ **3.5.3** 순서로 다운로드
  * //library 사용을 위해 3.5.1에서 3.5.3으로 변경
* **R studio [링크](https://www.rstudio.com)**
  * download -> free ver. -> RStudio Desktop 1.3.1093 (171.62MB)을 **default**로 install

| 데이터 파악 함수  | 예시          | 설명                                            |
| ----------------- | ------------- | ----------------------------------------------- |
| View(데이터 세트) | View(exdata1) | View 창을 통해 exdata1 데잍 세트의 데이터 확인  |
| str(데이터 세트)  | str(exdata1)  | exdata1 데이터 세트에 있는 변수의 속성 확인     |
| dim(데이터 세트)  | dim(exdata1)  | exdata1 데이터 세트의 데이터 프레임 확인(행 열) |
| ls(데이터 세트)   | ls(exdata1)   | exdata1 데이터 세트의 변수 항목을 리스트로 만듦 |



### 1-2. 설정

* Tools -> Global Options -> 저장소 C:\R
* Code -> Saving 에서 ASK->UTF-8
* **Project** 생성
  * new project -> New Directory -> New Project -> Browse -> C:\R
* 새로운 **File** 생성
  * File -> New File -> R Script 
  * `ctrl` + `shift` + `n`
* Eclipse와 같이 *.Rproj(project) 안에 *.R(class) 파일들이 위치한다
* **library**의 위치 
  * c:\Program Files\R\R-3.5.1\library
  * 문서\R\win-library\3.5

#### KoNLP (library 설치)

1. rstudio 끄기
   아래 사이트에서 Rtools35.exe 설치
   설치 시 기본 옵션이외의 설치 옵션 모두 클릭
   **https://cran.r-project.org/bin/windows/Rtools/history.html**

2. rstudio 켜기
   **install.packages("multilinguer")**

3. 의존성 설치
   **install.packages(c("hash", "tau", "Sejong", "RSQLite", "devtools", "bit", "rex", "lazyeval", "htmlwidgets", "crosstalk", "promises", "later", "sessioninfo", "xopen", "bit64", "blob", "DBI", "memoise", "plogr", "covr", "DT", "rcmdcheck", "rversions"), type = "binary")**

4. Git를 통해 KoNLP를 다운 받기 

* install
  * git 연결 프로그램 다운로드
    **install.packages("remotes")**
  * KoNLP 설치
    **remotes::install_github('haven-jeon/KoNLP', upgrade = "never", INSTALL_opts=c("--no-multiarch"))**



### 1-3. 단축키

* `ctrl` + `enter` : 실행
  * `ctrl`+`Alt`+`r` : 전체 실행
* 화면 크기 조절
  * `ctrl`+`shift`+`+` : 화면 크게
  * `ctrl`+`tab`+`-` : 화면 작게
* 콘솔 이동
  * `ctrl`+`1` : script 창으로
  * `ctrl`+`2` : console 창으로
* `ctrl`+`shift`+`n` : 새로운 script
* `ctrl`+`d` : 해당 행 삭제
* `ctrl`+`shift`+`c` : 주석 처리(#)
* `alt`+`-` : 화살표(<-) 작성



### 1-4. 개념

* R은 **script language**이기 때문에 ;을 찍는게 좋지만 마지막에는 ;을 찍지 않는다.

  * txt 파일을 사용할 때는 마지막 문장에서 `enter` 후 저장

  

 * |      | 2열               |      |      |
   | ---- | ----------------- | ---- | ---- |
   |      | 2열               |      |      |
   | 3행  | (3행2열)데이터 값 | 3행  | 3행  |
   |      | 2열               |      |      |

* 변수 만들기

  * **c()** : combine의 약어, 여러 데이터 값이 필요할 때 사용
  * **1:10**  : 1에서 10까지 1씩 증가하는 연속 값을 사용
  * **seq()** : sequence의 약어, 일정한 간격의 여러 숫자
    * x <- seq(10, 15) : 10~15의 연속된 숫자
    * x <- seq(1, 15, by = 4) : 1에서 15까지 4씩 증가
    * x <-seq(1,10,2) : 1에서 10까지 2씩 증가

  

## 2. 데이터 타입

* **데이터 구조**

  * **스칼라, 팩터** : 1차원 변수

  * **벡터** : 배열 c

  * **리스트** : k,v

  * **행렬** : 동일 타입의 matrix

  * **배열** : 다차원 행렬

  * **데이터 프레임** : 다중(데이터타입) 행렬

* 함수 사용의 예

  ```R
  v1 <- c(1,2,3,4,5)
  v
  v3 <-seq(1,10,2)
  ```

  



### 2-1. 스칼라(Scalar)

* R에서 데이터 타입의 기본은 벡터이고, 스칼라 데이터는 **길이가 1인 벡터**와 같은 것으로 볼 수 있다
* R에서는 문자열을 `""`과 `''` 중 어느 것으로 묶어도 된다

#### NA와 NULL

* R과 다른 언어의 가장 큰 차이 중 하나가 바로 NANot Available 상수다.

* **is.na()** 함수를 이용해 변수에 NA값이 저장되어 있는지 확인 할 수 있다

* **NA**는 결측치, 즉 데이터를 알지 못하거나 실수로 실수로 빼놓은 경우에 값이 없다고 나타내는 경우에 사용한다

* **NULL**은 편의를 위해 미정된 값을 표현하는 개념이다

  ```R
    is_even <- NULL
    if (a 가 짝수면) {
       is_even <- TRUE
    } else {
       is_even <- FALSE
    }
  ```




### 2-2. 팩터(Factor)

* **팩터Factor**는 범주형Categorical 데이터(자료)를 표현하기 위한 데이터 타입이다. ~~사실 이것만 보면 무슨 말인지 모르겠다~~

* 이해를 돕기 위해 예시를 보자

  ```R
  > 성별 <- factor("m", c("m", "f"))
  > 성별
  [1] m
  Levels: m f
  # 성별의 factor은 m으로 지정하고 factor가 담을 수 있는 # 값의 레벨은 "m", "f"로 제한되었다
  ```

  

### 2-3. 벡터(Vector)

* 벡터는 **c( )**를 사용해 생성하고, **names( )**를 사용해 이름을 부여할 수 있다. 아래에 벡터 관련 함수를 정리했다.

* 벡터는 **중첩할 수 없다**. 따라서 벡터 안에 벡터를 생성하면 단일 차원의 벡터로 변경된다. 중첩된 구조가 필요하다면 역시 리스트를 사용해야 한다.

  ```r
  > c(1, 2, 3, c(1, 2, 3))
  [1] 1 2 3 1 2 3
  ```

* 각 셀에는 **names() <-** 함수를 사용해 이름을 부여할 수 있다

  ```R
  > x <- c(1, 3, 4)
  > names (x) <- c("kim", "seo", "park")
  > x
  kim seo park
  1 3 4
  ```

* 벡터는 [ ] 안에 색인을 적어 각 요소를 가져올 수 있다. 이때, 색인은 다른 언어와 달리 **1부터 시작**한다.

  ```R
  > x <- c("a", "b", "c")
  > x[1]
  [1] "a"
  > x[3]
  [1] "c"
  ```

* ‘-색인’ 형태로 음의 색인을 사용해 **특정 요소만 제외**할 수도 있다.

  ```R
  > x <- c("a", "b", "c")
  > x[-1]
  [1] "b" "c"
  > x[-2]
  [1] "a" "c"
  ```

* **여러 위치에 저장된 값을 한 번에 가져오려면** ‘벡터명[색인 벡터]’ 형식을 사용한다.

  ```R
  > x <- c("a", "b", "c")
  > x[c(1, 2)]
  [1] "a" "b"
  > x[c(1, 3)]
  [1] "a" "c"
  ```

* **‘start:end’** 형태의 문법은 start부터 end까지의 숫자를 저장한 숫자 벡터를 뜻한다. 따라서 x[start:end]를 사용해 start부터 end까지의 데이터(start와 end에 위치한 요소 포함)를 볼 수 있다.

  ```R
  > x <- c("a", "b", "c")> x[1:2][1] "a" "b"> x[1:3][1] "a" "b" "c"
  ```

* 벡터의 각 셀에 names( )를 사용해 이름을 부여했다면, 이 이름을 사용해 데이터를 접근할 수 있다.

  ```R
  > x <- c(1, 3, 4)> names (x) <- c("kim", "seo", "park")> xkim seo park1 3 4> x["seo"]seo3> x[c("seo", "park")]seo park3 4
  ```

* 벡터의 **연산 함수**

  |  함수명   |            역할            | 반환값 |
  | :-------: | :------------------------: | :----: |
  | identical |   객체가 동일한지를 판단   | T / F  |
  |   union   |           합집합           | 합집합 |
  | intersect |           교집합           | 교집합 |
  |  setdiff  |           차집합           | 차집합 |
  | setequal  | x와 y가 같은 집합인지 판단 | T / F  |

* 벡터의 **연산자**

  |    연산자    |                   의미                    |
  | :----------: | :---------------------------------------: |
  | value %in% x |  벡터 x에 value가 저장되어 있는지 판단함  |
  |    x + n     | 벡터 x의 모든 요소에 n을 더한 벡터를 구함 |

* == 또는 != 연산자를 사용해 두 벡터에 저장된 값들을 한 번에 비교할 수 있다. 그러나 흔히 if 문 등의 조건문에서는 단 하나의 참 또는 거짓 값을 사용해야 하기 때문에 ==, !=가 아니라 앞서 설명한 identical( )을 사용해야 한다

  ```R
  > c(1, 2, 3) == c(1, 2, 100)
  [1] TRUE TRUE FALSE
  > c(1, 2, 3) != c(1, 2, 100)
  [1] FALSE FALSE TRUE
  ```

* 주어진 값을 반복하는 **rep** 함수

  ```R
  > rep(1:2, times=5)
  [1] 1 2 1 2 1 2 1 2 1 2
  
  > rep(1:2, each=5)
  [1] 1 1 1 1 1 2 2 2 2 2
  
  > rep(1:2, each=5, times=2)
  [1] 1 1 1 1 1 2 2 2 2 2 1 1 1 1 1 2 2 2 2 2
  ```

  

### 2-4. 리스트(List)

* 리스트는 ‘**(키, 값)**’ 형태의 데이터를 담는 연관 배열(Associative Array)이다

  ```R
  > (x <- list(name="foo", height=c(1, 3, 5)))$ name[1] "foo"$ height[1] 1 3 5
  ```

* 리스트 데이터 접근 문법

  | 문법  | 의미                                   |
  | ----- | -------------------------------------- |
  | x$key | 리스트 x에서 키 값 key에 해당하는 값   |
  | x[n]  | 리스트 x에서 n번째 데이터의 서브리스트 |
  | x     | 리스트 x에서 n번째 저장된 값           |

  ```R
  > x <- list(name="foo", height=c(1, 3, 5))> x$name[1] "foo"> x$height[1] 1 3 5> x1[1] "foo"> x2[1] 1 3 5
  ```

  

### 2-5. 행렬(Matrix)

* R의 행렬Matrix은 수학 시간에 배운 행렬의 정의와 같이 행(로우), 열(컬럼)의 수가 지정된 구조다. 벡터와 마찬가지로 행렬에는 **한 가지 유형의 스칼라만 저장**할 수 있다. 따라서 모든 요소가 숫자인 행렬은 가능하지만, ‘1열은 숫자, 2열은 문자열’과 같은 형태는 불가능하다

* **byrow=TRUE**

  * 리스트에서 데이터의 **방향 변경** 

* **행렬 생성 함수**

  ```R
  # rownames<- : 행렬의 행 이름을 설정한다.rownames(   x           # 2차원 이상의 행렬과 유사한 객체 ) <- value  # NULL 또는 x와 같은 길이의 문자열 벡터# colnames<- : 행렬의 열 이름을 설정한다.colnames(    x           # 2차원 이상의 행렬과 유사한 객체 ) <- value  # NULL 또는 x와 같은 길이의 문자열 벡터
  ```

* 행렬은 잘 사용하지 않는다

* **A %*% B** (곱할 때는 주의!)

* **dim**(dimension) : 객체의 차원 수를 구한다.

### 2-6. 배열(Array)

* 행렬이 2차원 데이터라면 배열Array은 **다차원 데이터**다. 예를 들어, 2×3 차원의 데이터를 행렬로 표현한다면 2×3×4 차원의 데이터는 배열로 표현한다.

### 2-7. 데이터 프레임(Data Frame)

* 데이터 프레임은 처리할 데이터를 마치 **엑셀의 스프레드시트와 같이 표 형태**로 정리한 모습을 하고 있다

* **nrow** : 행 개수 확인

* **str** : 임의의 R 객체의 내부 구조(structure)를 보인다

  | 문법           | 의미                                                         |
  | -------------- | ------------------------------------------------------------ |
  | d$colname      | 데이터 프레임 d에서 컬럼 이름이 colname인 데이터를 접근한다. |
  | d$colname <- y | 데이터 프레임 d에서 컬럼 이름이 colname인 컬럼에 데이터 y를 저장한다. 만약 colname이 d에 없는 새로운 이름이라면 새로운 컬럼이 추가된다. |

* 데이터 프레임에 기존에 없던 컬럼을 추가하고자 할 때도 같은 문법을 사용한다

  ```R
     x  y  z1  6  2  M2  7  4  F3  8  6  M4  9  8  F5 10 10  M> d$w <- c("A", "B", "C", "D", "E")> d   x  y  z  w1  6  2  M  A2  7  4  F  B3  8  6  M  C4  9  8  F  D5 10 10  M  E
  ```

* 데이터 프레임의 행 이름, 컬럼 이름은 각각 rownames( ), colnames( ) 함수로 지정할 수 있다.

  ```R
  > (x <- data.frame(1:3))  X1.31    12    23    3> colnames(x) <- c('val')> x  val1 12 23 3> rownames(x) <- c('a', 'b', 'c')> x  vala 1b 2c 3
  ```

* 다음과 같이 컬럼 이름을 지정할 수도 있다.

  ```R
  > d[, c("x", "y")]   x  y1  1  22  2  43  3  64  4  85  5 10> d[, c("x")][1] 1 2 3 4 5# drop 사용> d[, c("x"), drop=FALSE]  x1 12 23 34 45 5
  ```

* 데이터 프레임 접근 문법

  | 문법               | 의미                                                         |
  | ------------------ | ------------------------------------------------------------ |
  | d$colname          | 데이터 프레임 d의 컬럼 이름 colname에 저장된 데이터          |
  | d[m, n, drop=TRUE] | 데이터 프레임 d의 m행 n 컬럼에 저장된 데이터.m과 n을 벡터로 지정하여 다수의 행과 컬럼을 동시에 가져올 수 있으며 m, n에는 색인뿐만 아니라 행 이름이나 컬럼 이름을 지정할 수 있다. **만약 m, n 중 하나를 생략하면 모든 행 또는 컬럼의 데이터를 의미한다.** d[, n]과 같이 행을 지정하지 않고 특정 컬럼만 가져올 경우 반환 값은 데이터 프레임이 아니라 해당 컬럼의 데이터 타입이 된다. 이러한 형 변환을 원하지 않으면 drop=FALSE를 지정하여 데이터 프레임을 반환하도록 할 수 있다. |

* head() : 객체의 처음 부분을 반환한다

* View() : 데이터 뷰어를 호출한다

* **as.data.frame()** : 데이터를 분석하기 위해 데이터 프레임 형식으로 만들어주게 되는데, 이때 매우 많이 사용된다



## 3. 데이터 가공 (아직 안함)

### 데이터 수집

* library(readxl)

* read.csv :

  * ```R
    sh <- read.csv("shop2.txt",               header = T,               stringsAsFactors=F,               fileEncoding = "UTF-8")
    ```

* read_excel : xls, xlsx 엑셀 파일을 가져옴

  * ```R
    ex1 <- read.table(file = "mydata.xls", header=T, sep=",",stringsAsFactors = F)# table을 이용한 이런 방식도 가능하다
    ```

* read.table : txt 파일 가져옴

  * header : 1행이 변수명인지 아닌지 판단 (header = TRUE)
  * skip : 특정 행까지 제외하고 가져오기 (skip = 2)
  * nrows : 특정 행까지 데이터 가져오기 (nrows = 7)
  * sep : 데이터 구분자 지정 (sep = ",")

* rda 파일

  * save() : save(데이터세트, file = "파일명")
  * load() : load(위치)

* csv, txt 파일

  * write.csv(데이터테이블, file = "C:/Rstudy/data_ex.csv")
  * write.table(데이터테이블, file = "C:/Rstudy/data_ex.txt")



### 데이터 가공

| 산술 연산자 |  기능  |
| :---------: | :----: |
|      +      | 더하기 |
|      -      |  빼기  |
|      *      | 곱하기 |
|      /      | k누기  |
|     %/%     |   몫   |
|     %%      | 나머지 |
|  ** 또는 ^  |  제곱  |

#### ifelse 사용

* ```R 
  sh$AGE_HL <- ifelse(sh$AGE >= 30, "H",                    ifelse(sh$AGE >= 25, "M",                           ifelse(sh$AGE >= 20, "L","F")                           )                    )# 총 구매 금액(PRICE*QT)이 10,000 이하면 "B"# 30,000 이하면 "S", 100,000 이하면 "G" 를 GRADE 컬럼으로 만드시오sh$GRADE <- ifelse(sh$PRICE * sh$QT <= 10000, "B",                   ifelse(sh$PRICE * sh$QT <= 40000, "S",                          ifelse(sh$PRICE * sh$QT <= 100000, "G","F")                   ))
  ```



#### Join 함수

```R
library(readxl)y16 <- as.data.frame(read_excel("y16.xlsx"))y17 <- as.data.frame(read_excel("y17.xlsx"))--left_joinbind_col1 <- left_join(y16,y17,by="ID")bind_col2 <- left_join(y17,y16,by="ID")# 왼쪽에 오는 차트에 ID가 없으면 표에 나타내지 않는다--inner_joininner_col1 <- inner_join(y16,y17,by="ID")inner_col2 <- inner_join(y17,y16,by="ID")# 둘이 공통적으로 갖고있지 않은 ID의 내용은 나타내지 않는다--full_joinfull_col1 <- full_join(y16,y17,by="ID")full_col2 <- full_join(y17,y16,by="ID")# 하나라도 ID를 가지고 있으면 그 내용을 나타낸다. 단, 앞에 오는 표에 ID가 없으면# default로 적용되던 ID ascending이 적용되지 않고 그 뒤에 이어서 나타낸다
```



#### rowSums, rowMeans

```R
# 예시sht <- load("shoptotal.rda")tt <- sum(sh$TOTAL)tt <- rowSums(sh[,c(4:7)])tt <- rowSums(sh[,c(4:7)], na.rm = T)# 16,17년 결제 금액의 합과 건수의 합을 테이블에 추가하시오# SUM_AMT, SUM_CNTbind_col2$SUM_AMT <- rowSums(bind_col2 %>% select(AMT17,AMT16), na.rm = T)bind_col2$SUM_CNT <- rowSums(bind_col2 %>% select(Y17_CNT,Y16_CNT), na.rm = T)
```



### 실습

```R
p.150~151# Q01# 중간고사 성적 파일(mid_exam.xlsx)을 mid_exam에 저장# 수학점수를 MATH_MID, 영어점수를 ENG_MID로 변경library(readxl)library(dplyr)mid_exam <- as.data.frame(read_excel("day02/mid_exam.xlsx"))mid_exam <- rename(mid_exam, MATH_MID = MATH,                   ENG_MID = ENG)# Q02# 기말고사 성적 파일(final_exam.xlsx)을 final_exam에 저장# 수학점수를 MATH_FINAL, 영어점수를 ENG_FINAL로 변경final_exam <- as.data.frame(read_excel("day02/final_exam.xlsx"))final_exam <- rename(final_exam, MATH_FINAL = MATH,                   ENG_FINAL = ENG)# Q03# 중간고사와 기말고사 점수가 모두 있는 데이터를 total_exam에 결합합 total_exam <- inner_join(mid_exam,final_exam,by="ID")# Q04# total_exam을 활용해 수학 점수 평균을 MATH_AVG, 영어 점수 평균을을 ENG_AVG에 구함total_exam$MATH_AVG <- rowMeans(total_exam %>% select(MATH_MID,MATH_FINAL), na.rm = T)total_exam$ENG_AVG <- rowMeans(total_exam %>% select(ENG_MID,ENG_FINAL))# inner_join으로 결합했기 때문에 na값이 없다, na.rm=T 없어도 된다;# Q05# 성적이 모두 입력된 9명 학생의 평균을 TOTAL_AVG에 구함# total_exam$TOTAL_AVG <- rowSums(total_exam %>% filter(is.na(MATH_FINAL) == F) %>% select(MATH_MID,MATH_FINAL))# 처음에 실수로 모든 학생의 데이터를 가져옴, 중간기말 성적이 모두 있는 학생으로 변경# full_join -> inner_join;total_exam$TOTAL_AVG <- rowMeans(total_exam %>% select(MATH_AVG,ENG_AVG))# Q06# 수학 점수와 영어 점수의 전체 평균을 구해본다total_exam %>% summarize(MATOT_AVG=mean(MATH_AVG),                         ENGTOT_AVG=mean(ENG_AVG))# Q07# 중간고사 수학 80점 이상, 중간고사 영어 90점 이상인 학생을 선별total_exam %>% filter(total_exam$MATH_MID >= 80 & total_exam$ENG_MID>= 90)# Q08# 수학 점수 평균과 영어 점수 평균에 대한 상자 그림boxplot(total_exam$MATH_AVG, total_exam$ENG_AVG, ylim = c(40,100), main = "점수",         names=c("수학 평균", "영어 평균"), col = c("red", "blue"))
```



## 4. 패키지

* 주요 패키지와 포함 함수

| 분류   | 기능                                 | 패키지명                       |
| ------ | ------------------------------------ | ------------------------------ |
| 로드   | 데이터베이스 처리                    | RMySQL, RPostgressSQL, RSQLite |
|        | 엑셀 처리                            | XLConnect, readxl              |
| 가공   | 데이터 조작                          | dplyr                          |
|        | 데이터 레이아웃 변환                 | reshape2                       |
|        | 문자열 처리                          | stringr                        |
| 시각화 | 그래프                               | ggplot2                        |
|        | 워드 클라우드                        | wordcloud, wordcloud2          |
|        | 구글 차트                            | googleVis                      |
| 모델링 | 선형 혼합 효과 모형                  | Ime2                           |
|        | 비선형 혼합 효과 모형                | nlme                           |
|        | 머신러닝 랜덤 포레스트               | randomForest                   |
|        | 범주형 데이터 시각화                 | vcd                            |
|        | Lasso, elastic-net 회귀분석          | glmnet                         |
|        | 생존 분석                            | survival                       |
|        | 회귀분석 및 분류 모델의 트레이닝     | caret                          |
| 리포트 | 웹 대시보드                          | shiny                          |
| 공간   | 지리 지도                            | maps                           |
|        | 구글 지도                            | ggmap                          |
| 기타   | 대용량 텍스트 데이터 처리            | data.table                     |
|        | 멀티코어 사용으로 병렬 프로세싱 처리 | parallel                       |
|        | XML 문서 처리                        | XML                            |
|        | JSON 데이터 처리                     | jsonlite                       |
|        | HTTP Connection                      | httr                           |

  

* 패키지의 **설치**와 **사용 방법**
  * 설치 : install.package("package")
  * 사용 : library(package)
  * win-library를 다른 환경의 동일 경로로 옮기면 windows에서는 그 패키지들을 사용 가능(폴더 형태)



* **descr 패키지**

  ```R
  install.packages("descr")library(descr)fq <- freq(r5$AREA, plot = F)# plot는 막대 그래프를 보여주냐 마냐 설정, plot 설정 안해주면 T가 default
  ```

  

### 4-1. reshape2

* 데이터의 행을 열로, 열을 행으로

#### melt()

* melt(데이터 세트, id.var = "기준열", measure.var = "변환열")

  ```R
  > library(readxl)> score <- read.table("h.txt",     fileEncoding = "UTF-8", sep=",", header = T)> score <- as.data.frame(score)> score 이름 국어 영어 컴퓨터1 민철  100   80     852 지수   70   70    1003 지영   50  100     80> library(reshape2)> melt(score, id.vars = "이름", measure.vars = 	c("국어", "컴퓨터"))  이름 variable value1 민철     국어   1002 지수     국어    703 지영     국어    504 민철   컴퓨터    855 지수   컴퓨터   1006 지영   컴퓨터    80
  ```

* **dim()**, **str()** 을 이용해 table 구조 파악

* **names(score) <- tolower(names(score))**

  * [1] "이름"   "국어"   "영어"   "컴퓨터"

    header가 영어일 경우, 이를 보여주는 함수를 이용해 하나로 통일시켜 헷갈리는 것을 방지

  

#### dcast()

* dcast(데이터 세트, **기준열** ~ **변환열**)
  * 데이터 프레임(Data Frame 형태를 변환)

```R
> exam2 <- dcast(exam1, 이름~과목)  이름    과목   점수             이름  국어  컴퓨터     1 민철    국어   100           1 민철  100   852 지수    국어    70      -->  2 지수   70   1003 지영    국어    50           3 지영   50   804 민철   컴퓨터   855 지수   컴퓨터   1006 지영   컴퓨터   80# 1-1. 바로 <NA>값 있는 데이터 지우기m_air <- melt(airquality, id.vars = c("month","day"), na.rm=T)# 1-2. 2번에 걸쳐서 지워주기m_air <- melt(airquality, id.vars = c("month","day"))m_air$value = ifelse(is.na(m_air$value), 0, m_air$value)## 2. month와 day를 기준열로 두고 dcast 함수 호출n_air <- dcast(m_air, month + day ~ variable)
```



#### acast()

* acast(데이터 세트, **기준열** ~ **변환열** ~ **분리기준열**)

```R
> library(reshape2)> airquality> names(airquality) <- toupper(names(airquality))> m_air <- melt(airquality, id.vars = c("MONTH","DAY"), na.rm = T)> names(m_air) <- toupper(names(m_air))> a_air <- acast(m_air, MONTH ~ MONTH ~ VARIABLE), , OZONE   5 6  7  8  95 26 0  0  0  06  0 9  0  0  07  0 0 26  0  08  0 0  0 26  09  0 0  0  0 29, , SOLAR.R   5  6  7  8  95 27  0  0  0  06  0 30  0  0  07  0  0 31  0  08  0  0  0 28  09  0  0  0  0 30, , WIND   5  6  7  8  95 31  0  0  0  06  0 30  0  0  07  0  0 31  0  08  0  0  0 31  09  0  0  0  0 30, , TEMP   5  6  7  8  95 31  0  0  0  06  0 30  0  0  07  0  0 31  0  08  0  0  0 31  09  0  0  0  0 30> a_air <- acast(m_air, MONTH ~ VARIABLE, mean)# mean과 sum 모두 가능    OZONE   SOLAR.R   WIND      TEMP5 23.61538 181.2963 11.622581 65.548396 29.44444 190.1667 10.266667 79.100007 59.11538 216.4839  8.941935 83.903238 59.96154 171.8571  8.793548 83.967749 31.44828 167.4333 10.180000 76.90000
```



### 4-2. KoNLP (Korean Natural Language Process)

#### 설치

1. rstudio 끄기
   [알툴즈 사이트](https://cran.r-project.org/bin/windows/Rtools/history.html)에서 **Rtools35.exe** 설치 (R 버전에 맞게)
   설치 시 기본 옵션이외의 **설치 옵션 모두** 클릭

2. rstudio 켜기
   **install.packages("multilinguer")**

3. 의존성 설치
   **install.packages(c("hash", "tau", "Sejong", "RSQLite", "devtools", "bit", "rex", "lazyeval", "htmlwidgets", "crosstalk", "promises", "later", "sessioninfo", "xopen", "bit64", "blob", "DBI", "memoise", "plogr", "covr", "DT", "rcmdcheck", "rversions"), type = "binary")**

4. Git를 통해 KoNLP를 다운 받기 

   4-1. git 연결 프로그램 다운로드
   **install.packages("remotes")**

   4-2. KoNLP 설치
   **remotes::install_github('haven-jeon/KoNLP', upgrade = "never", INSTALL_opts=c("--no-multiarch"))**

5. **wordcloud** 설치

   * 경우에 따라 wordcloud와 wordcloud2 둘 다 사용한다

#### 사전 설정

```R
library(KoNLP) # KoNLP 패키지 로드
useSystemDic() # 시스템 사전 설정
useSejongDic() # 세종 사전 설정
useNIADic() # NIADic 사전 설정

# 분석 단계
# 텍스트 수집 => 분해 => 단어 추출 => 정제 => 정형 데이터 생성 => 분석 => 시각화
```



#### 단어 추가

```R
add_wd <- c("코비드","코비드19","코로나")
buildDictionary(user_dic = data.frame(
  add_wd,rep("ncn",length(add_wd))
), replace_usr_dic = T)
# replace_usr_dic 옵션은 추가하는 단어들을 기존 사전의 내용과 상관없이 대체해서 사용한다는 의미
```



#### 형태소 분석과 응용

```R
# 데이터 가져오기wd <- readLines("wc.txt", encoding = "UTF-8")# 단어가 추가된다. USE.NAMES는 열 이름을 나타내는 옵션wd2 <- sapply(wd, extractNoun, USE.NAMES = F)# wd2를 벡터로 변환 후 lwd 변수에 할당lwd <- unlist(wd2)# lwd에서 2글자가 넘는 단어만 가져와 lwd2에 넣는다lwd2 <- Filter(function(x){  nchar(x) >= 2} , lwd)# lwd2의 단어 빈도수를 wc에 넣는다 (몇번 나왔는지 나옴)wc <- table(lwd2)# wd2 에 있는 "선언"이라는 단어를 "Hi"로 바꾸고 lwd에 넣는다lwd <- gsub("선언", "Hi", wd2)# 여러 번 반복해 필요 없는 단어들을 모두 지워보자lwd <- gsub("[0-9]","",lwd)lwd <- gsub("[A-Z]","",lwd)lwd <- gsub("[a-z]","",lwd)# 보고 싶은 단어의 갯수 지정wc <- head(sort(wc, decreasing = T),100)
```



#### wordcloud2 사용

* wordcloud2(wc) ==> 단어를 횟수에 따라 워드클라우드 그림으로 나타내준다

  * wordcloud2(wc, color = "random-light", backgroundColor = "black")

* 옵션

  * **minSize** : 시각화할 최소 빈도 수 설정
  * **size** : 배수 기준 워드클라우드 크기 변경
  * **col** : 색상 설정
  * **rotateRatio** : 회전율
  * **backgroundColor** : 배경색
  * **figPath** : 이미지

  

```R
library(wordcloud2)		# wordcloud2 패키지 로드wordcloud2(word_table2) # word_table2의 워드 클라우드 설정# color(색상)과 backgroundColor(배경색) 옵션 사용wordcloud(word_table2, color = "random-light", backgroundColor = "black")# fontFamily(글꼴), size(크기), shape(모양) 옵션 사용wordcloud2(word_table, fontFamily = "맑은 고딕", size = 1.2, color = "random-light",           backgroundColor = "black", shape = "star")## demoFreq의 사용 (가볍게 지나간 부분, 필요에 의해 사용될 수 있다)# 선택된 색상만 반복되는 워드클라우드wordcloud2(demoFreq, size = 1.6, color = rep_len(c("red", "blue"), nrow(demoFreq)))# 일정한 방향으로 정렬된 워드 클라우드wordcloud2(demoFreq, minRotation = -pi / 6, maxRotation = -pi / 6, rotateRatio = 1)# 원하는 이미지에 워드클라우드 표시하기wordcloud2(demoFreq, figPath = "peace.png")
```



#### wordcloud 사용

* wordcloud2 사진을 저장하고 싶은데 wordcloud로만 되어 사용하게 됨

1.  **jpeg()** 으로 미리 빈 이미지 파일(0KB)을 만들고 **dev.off()**하며 저장된다

```R
library(wordcloud)library(tm)library(RCurl)library(RColorBrewer)jpeg(filename = "1p.jpg",width = 300, height = 300,     quality = 120)  # 빈 파일과 설정을 만든다palate <- brewer.pal(9,"Set1")wordcloud(names(wc),          freq=wc,          scale=c(5,0,5),          rot.per = 0.35,          min.freq = 1,          random.color = T,          random.order = F,          colors=palate          )dev.off()# dev.off() 없으면 저장되다가 만다!
```



2. **windows()**으로 창을 열어 이미지를 받아온 후 **savePlot()**으로 저장한다

   미리보기 가능, 저장은 windows() 하나에 한번만 가능

```R
library(wordcloud)library(tm)library(RCurl)library(RColorBrewer)palate <- brewer.pal(9."Set1")windows()wordcloud(names(wc),          freq=wc,          scale=c(5,0,5),          rot.per = 0.35,          min.freq = 1,          random.color = T,          random.order = F,          colors=palate          )savePlot("1p.png",type="png")
```



### 4-3. dplyr

* install 중 오류가 나면 **install.packages("Rcpp")** 먼저 해준다

| 함수명      | 기능                      | 함수명        | 기능                     |
| ----------- | ------------------------- | ------------- | ------------------------ |
| filter()    | 조건에 맞는 행 추출       | distinct()    | 중복 값 제거             |
| arrange()   | 지정한 여을 기준으로 정렬 | group_by()    | 데이터 그룹 생성         |
| select()    | 열 추출                   | sample_n()    | n개의 샘플 추출          |
| mutate()    | 열 추가                   | sample_frac() | n% 비율의 샘플 추출      |
| summarize() | 데이터 요약               | %>%           | 파이프 연산자, 함수 연결 |



#### rename()

* 테이블 header의 내용을 바꿔준다

```R
  TX_ID  TX_NM TX_A TX_T  TX_P TX_Q
1  id01 이말숙   23   15 10000    1
2  id02 김말숙   28   NA 20000    2
3  id03 홍말숙   30   15 30000    3

> sh <- rename(sh, ID=TX_ID,
       NAME=TX_NM,
       AGE=TX_A,
       TEMP=TX_T,
       PRICE=TX_P,
       QT=TX_Q
       ) 

    ID   NAME  AGE TEMP PRICE   QT
1  id01 이말숙   23   15 10000    1
2  id02 김말숙   28   NA 20000    2
3  id03 홍말숙   30   15 30000    3
```



#### filter()

* filter(데이터 세트, 조건절)

```r
> filter(mtcars, cyl == 4)    mpg cyl  disp  hp drat    wt  qsec vs am gear carb1  22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    12  24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    23  22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    24  32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    15  30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    26  33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    17  21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    18  27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    19  26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    210 30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    211 21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2> filter(mtcars, cyl>=6 & mpg>20)   mpg cyl disp  hp drat    wt  qsec vs am gear carb1 21.0   6  160 110 3.90 2.620 16.46  0  1    4    42 21.0   6  160 110 3.90 2.875 17.02  0  1    4    43 21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
```



#### arrange()

* arrange(데이터 세트, 열1, 열2, ...) 
* 오름차순으로 정렬할 때 사용
  * 내림차순은 arrange(데이터 세트, desc(열1))



#### select()

* filter() 함수는 행을 추출할 때 사용하지만, select()함수는 지정한 변수(열) 추출할 때 사용한다

```r
> head(select(mtcars, am, gear))Mazda RX4          1    4Mazda RX4 Wag      1    4Datsun 710         1    4Hornet 4 Drive     0    3Hornet Sportabout  0    3Valiant            0    3
```



#### mutate()

* mutate(데이터 세트, 추가할 열 이름 = 조건1, ...)
* 새로운 열을 추가하고 결과를 할당한다

```r
> head(mutate(mtcars, years = "1974"))   mpg cyl disp  hp drat    wt  qsec vs am gear carb years1 21.0   6  160 110 3.90 2.620 16.46  0  1    4    4  19742 21.0   6  160 110 3.90 2.875 17.02  0  1    4    4  19743 22.8   4  108  93 3.85 2.320 18.61  1  1    4    1  19744 21.4   6  258 110 3.08 3.215 19.44  1  0    3    1  19745 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2  19746 18.1   6  225 105 2.76 3.460 20.22  1  0    3    1  1974
```



#### distinct()

* 중복된 값을 제거할 수 있다
* distinct(데이터 세트, 열1, ...)

```r
> distinct(mtcars, cyl)
  cyl
1   6
2   4
3   8

> distinct(mtcars, cyl, gear)
  cyl gear
1   6    4
2   4    4
3   6    3
4   8    3
5   4    3
6   4    5
7   8    5
8   6    5
```



#### summarise()

* 통계 함수인 mean, sum, median, min, max과 함께 사용한다
* 개수를 구하는 **n()** 함수와 함께 사용하기도 한다

```R
> summarise(mtcars, cyl_mean = mean(cyl), cyl_min = min(cyl), cyl_max = max(cyl))
  cyl_mean cyl_min cyl_max
1   6.1875       4       8
> summarise(mtcars, mean(cyl), min(cyl), max(cyl))
  mean(cyl) min(cyl) max(cyl)
1    6.1875        4        8
```



#### group_by()

* 그룹별 데이터를 요약할 때 사용하며, **summarise()** 함수와 **n()**을 이용해 같이 사용하기도 한다

```r
> gr_cyl <- group_by(mtcars, cyl)> summarise(gr_cyl, n())# A tibble: 3 x 2    cyl `n()`  <dbl> <int>1     4    112     6     73     8    14
```



#### sample_n()

* sample_n(데이터 세트, 추출할 샘플 개수)

#### sample_frac()

* sample_frac(데이터 세트, 추출할 샘플 비율)
  * sample_frac(mtcars, 0.2) ==> 20% 추출



#### %>%

```r
> group_by(mtcars, cyl) %>% summarise(n())# A tibble: 3 x 2    cyl `n()`  <dbl> <int>1     4    112     6     73     8    14
```



### 4-4. ggplot2

#### ggplot

* **ggplot(데이터 세트, aes(데이터 속성))**
  * aes는 **좌표축**과 **축 이름**

```R
library(ggplot2)# jpeg(filename = "gg1.jpg", width = 300, height = 300, quality = 120)ggplot(airquality, aes(x = Day, y = Temp))# dev.off()# 샵 풀고 실행하면 아래와 같은 사진이 저장된다
```

![ggplot1](md-images/ggplot1.png)

* **geom_point()**

```R
ggplot(airquality, aes(x = DAY, y = TEMP)) + geom_point(size = 3, color = "red")# size와 color 없이 'geom_point()'만 입력하면 default 설정으로 나타난다
```

![ggplot2](md-images/ggplot2.png)

* **geom_line()**

```R 
ggplot(airquality, aes(x = DAY, y = TEMP)) + geom_line()
# line도 size와 color 변경이 가능하지만 default 값이 더 이쁘다
```

![ggplot3](md-images/ggplot3.png)

* **geom_point() + geom_line()**

```R
ggplot(airquality, aes(x = DAY, y = TEMP)) + geom_line() + geom_point()
# geom과 line을 같이 사용하면 더 좋다
```

![ggplot4](md-images/ggplot4.png)



* **geom_bar()**

```R
ggplot(mtcars, aes(x = cyl)) + geom_bar(width = 0.5)
# 막대 그래프
```

![geombar](md-images/geombar.png)



* **geom_bar() 누적**

```R
ggplot(mtcars, aes(x = factor(cyl))) + geom_bar(aes(fill = factor(gear)))
# fill을 통해 각 레벨 별로 몇개씩 cyl에 count되었는지 확인한다

# ggplot(mtcars, aes(x = factor(cyl))) + geom_bar(aes(fill = factor(gear))) 
# + coord_polar  을 사용하여 선버스트 차트로 나타낼 수도 있다
```

![geombar2](md-images/geombar2.png)

* **geom_boxplot()** : 분포와 심하게 벗어난 극단의 데이터까지 나타내줌(주식 봉)
* **geom_histogram()** : 도수 분포를 **기둥 모양 그래프**로 표현한 히스토그램을 만드는 함수
* **geom_abline()** : x축 또는 y축과 만나는 값인 **절편**과 **기울기**를 설정하여 그래프에 사선을 그린다
* **geom_hline()** : **평행선**을 그릴 때 사용하며, y 절편을 입력해야 한다
* **geom_vline()** : **수직선**을 그릴 때 사용하며, x 절편을 입력해야 한다
* **geom_text()** : 각 점에 **텍스트**로 표시해줄 때 사용한다
* **annotate()** : 그래프 위에 **사각형**이나 화살표 등으로 특정 영억을 강조할때 사용
* **labs()** : 축 이름과 그래프 제목을 나타내어 그래프 파악을 도움
* **theme()** : theme_gray() 등 그래프의 테마를 설정해줌

#### googleVis



#### ggmap

1. Package 설치

   -> git을 통해 설치받는다

   ```R
   install.package("devtools")
   library(devtools)
   install_github("dkahle/ggmap") # 깃허브에서 받아옴
   ```

2. Git에서 받아와 지도를 뿌린다

   ```R
   library(ggmap)
   library(ggplot2)
   
   googleAPIkey = "AIzaSyCf0OaMj_2ZLeKlOZO2alwc685pjyRf-Gs"
   # 지역의 위치 정보를 가져온 API Key를 저장
   register_google(googleAPIkey) # Google API Key 인증
   mm <- get_googlemap("mapogu", maptype = "roadmap", zoom=12)
   # mm에 구글맵을 받아서 저장함
   ggmap(mm)
   ```

3. 지도 위에 산점도 그리기

   ```R
   library(dplyr)
   geo_code <- enc2utf8("마포구") %>% geocode()
   # 한글 검색을 위해 utf8로 변환한 후 위도와 경도 데이터를 geo_code 변수에 할당
   geo_data <- as.numeric(geo_code) #리스트를 숫자로 변환하여 geo_data에 할당
   
   geo_googlemap(center = geo_data, maptype = "roadmap", zoom = 12) %>%
   		geom_point(data = geo.code, aes(x = geo_code$lon, y = geo_code$lat))
   # geo_code에 있는 경도(lon)와 위도(lat) 값으로 산점도 그리기
   ```

이어서, **p.252** 부터 실습



## 5. eclipse 연동

* Rserve를 통한 Java 연동1

```R 
install.package("Rserve")
Rserve::runRserve() # front에서 실행      ㅣ =>
Rserve::Rserve()    # background에서 실행 ㅣ => 둘 다 종료하려면 Rstudio를 강제 종료
Rserve::Rserve(args="--RS-enable-remote") # 

a1 <- function(){
  result <- c(1,2,3,4,5)
  return(result)
}
a2 <- function(){
  result <- c(3,4,5,4,5)
  return(result)
  }
a3 <- function(){
  result <- c(7,8,9,4,5)
  return(result)
}

func1 <- function(x){
  if(x == 10){
    return(a1())
  }else if(x == 20){
  return(a2())
  }else if(x == 30){
    return(a3())
  }
}

result <- func1(20)
result
```



```Java
// eclipse와 연결하려면 hive, oracle과 마찬가지로 library가 필요하다
// 네이버 카페에서 Rengine.jar과 RserveEngine.jar 다운로드

public class RTest {

	public static void main(String[] args) throws Exception {
		RConnection rconn = null;
		int arg = 20
		try {
			rconn = new RConnection("127.0.0.1");
			rconn.setStringEncoding("utf8");
			rconn.eval("source('C:/R/day04/f2.R',encoding='UTF-8')")  // 호출
			REXP rexp = rconn.eval("func1("+arg+")")
			int result [] = rexp.asIntegers();

			for(int i : result) {
				System.out.println(i);
		}
}			 catch (RserveException e) {
			e.printStackTrace();
		}
		System.out.println("Connection completed")

		rconn.close();
		}
	}
```



* Rserve를 통한 Java 연동2 (list 이용)

```R
a1 <- function(){
  result <- c(1,2,3,4,5)
  return(result)
}
a2 <- function(){
  result <- read.csv("test.txt")
  return(result)
  }
a3 <- function(){
  result <- c(7,8,9,4,5)
  return(result)
}

func1 <- function(x){
  if(x == 10){
    return(a1())
  }else if(x == 20){
  return(a2())
  }else if(x == 30){
    return(a3())
  }
}

result <- func1(20)
result
# a2 변경했음
```



```Java
public static void main(String[] args) throws Exception {
		RConnection rconn = null;
		int arg = 20;
		try {
			rconn = new RConnection("127.0.0.1");
			rconn.setStringEncoding("utf8");
			rconn.eval("source('C:/R/day04/f2.R',encoding='UTF-8')");
			REXP rexp = rconn.eval("func1("+arg+")");
//			int result [] = rexp.asIntegers();
//			for(int i : result) {
//				System.out.println(i);
//		}
			RList rlist = rexp.asList();
			String s1 [] = rlist.at("x").asStrings();
			int i1 [] = rlist.at("y").asIntegers();
			for(int i=0;i<s1.length;i++) {
				System.out.println(s1[i]+" "+i1[i]);
			}
			
			} catch (RserveException e) {
			e.printStackTrace();
		}
		System.out.println("Connection completed");

		rconn.close();
	}
	
}
```



## 6.실습 문제



## 7. 오류 예시

### treemap 사용 중 오류

```R
install.packages("shiny", type = "binary")
install.packages("devtools", type = "win.binary")
```



### encoding vs fileEncoding

* utf-8 일 때는 read.table 에서 fileEncoding
  아니면 encoding로 되는지 확인해보기
  --> 한글 사용을 위해 utf-8 사용하는 것

