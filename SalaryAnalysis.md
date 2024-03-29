107-2 大數據分析方法 作業一
================
put your name here

搞不清楚各行各業的薪資差異嗎? 念研究所到底對第一份工作的薪資影響有多大? CP值高嗎? 透過分析**初任人員平均經常性薪資**-
[開放資料連結](https://data.gov.tw/dataset/6647)，可初步了解台灣近幾年各行各業、各學歷的起薪。

## 比較103年度和106年度大學畢業者的薪資資料

### 資料匯入與處理

``` r
library(jsonlite)
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(readr)
pay103<-read_csv("C:/Users/edwardhung/Desktop/103.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   年度 = col_double(),
    ##   大職業別 = col_character(),
    ##   `經常性薪資-薪資` = col_double(),
    ##   `經常性薪資-女/男` = col_double(),
    ##   `國中及以下-薪資` = col_character(),
    ##   `國中及以下-女/男` = col_character(),
    ##   `高中或高職-薪資` = col_character(),
    ##   `高中或高職-女/男` = col_character(),
    ##   `專科-薪資` = col_character(),
    ##   `專科-女/男` = col_character(),
    ##   `大學-薪資` = col_character(),
    ##   `大學-女/男` = col_character(),
    ##   `研究所及以上-薪資` = col_character(),
    ##   `研究所及以上-女/男` = col_character()
    ## )

``` r
pay104<-read_csv("C:/Users/edwardhung/Desktop/104.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   年度 = col_double(),
    ##   大職業別 = col_character(),
    ##   `經常性薪資-薪資` = col_double(),
    ##   `經常性薪資-女/男` = col_character(),
    ##   `國中及以下-薪資` = col_character(),
    ##   `國中及以下-女/男` = col_character(),
    ##   `高中或高職-薪資` = col_character(),
    ##   `高中或高職-女/男` = col_character(),
    ##   `專科-薪資` = col_character(),
    ##   `專科-女/男` = col_character(),
    ##   `大學-薪資` = col_character(),
    ##   `大學-女/男` = col_character(),
    ##   `研究所及以上-薪資` = col_character(),
    ##   `研究所及以上-女/男` = col_character()
    ## )

``` r
pay105<-read_csv("C:/Users/edwardhung/Desktop/105.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   年度 = col_double(),
    ##   大職業別 = col_character(),
    ##   `經常性薪資-薪資` = col_double(),
    ##   `經常性薪資-女/男` = col_character(),
    ##   `國中及以下-薪資` = col_character(),
    ##   `國中及以下-女/男` = col_character(),
    ##   `高中或高職-薪資` = col_character(),
    ##   `高中或高職-女/男` = col_character(),
    ##   `專科-薪資` = col_character(),
    ##   `專科-女/男` = col_character(),
    ##   `大學-薪資` = col_character(),
    ##   `大學-女/男` = col_character(),
    ##   `研究所及以上-薪資` = col_character(),
    ##   `研究所及以上-女/男` = col_character()
    ## )

``` r
pay106<-read_csv("C:/Users/edwardhung/Desktop/106.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   年度 = col_double(),
    ##   大職業別 = col_character(),
    ##   `經常性薪資-薪資` = col_double(),
    ##   `經常性薪資-女/男` = col_character(),
    ##   `國中及以下-薪資` = col_character(),
    ##   `國中及以下-女/男` = col_character(),
    ##   `高中或高職-薪資` = col_character(),
    ##   `高中或高職-女/男` = col_character(),
    ##   `專科-薪資` = col_character(),
    ##   `專科-女/男` = col_character(),
    ##   `大學-薪資` = col_character(),
    ##   `大學-女/男` = col_character(),
    ##   `研究所及以上-薪資` = col_character(),
    ##   `研究所及以上-女/男` = col_character()
    ## )

``` r
for(n in 1:14){
  pay103[[n]]<-gsub("—|…|_|、","",pay103[[n]])
}
for(n in 1:14){
  pay104[[n]]<-gsub("—|…|_|、","",pay104[[n]])
}
for(n in 1:14){
  pay105[[n]]<-gsub("—|…|_|、","",pay105[[n]])
}
for(n in 1:14){
  pay106[[n]]<-gsub("—|…|_|、","",pay106[[n]])
}
```

### 106年度薪資較103年度薪資高的職業有哪些?

``` r
#這是R Code Chunk
payjoin<-inner_join(pay103,pay106,by="大職業別")
payjoin$薪資比<-as.numeric(payjoin$`大學-薪資.y`)/as.numeric(payjoin$`大學-薪資.x`)
payjoin<-arrange(payjoin,desc(薪資比))
payjoin<-na.omit(payjoin)
pp<-payjoin[payjoin$薪資比>1,]
head(pp,10)
```

    ## # A tibble: 10 x 28
    ##    年度.x 大職業別 `經常性薪資-薪資.x`~ `經常性薪資-女/男.x`~ `國中及以下-薪資.x`~
    ##    <chr>  <chr>    <chr>            <chr>            <chr>           
    ##  1 2014   其他服務業-技~ 24724            89.1             ""              
    ##  2 2014   住宿及餐飲業-~ 21647            98.45            20992           
    ##  3 2014   用水供應及污染~ 28552            99.46            ""              
    ##  4 2014   專業科學及技術~ 32078            99.63            ""              
    ##  5 2014   其他服務業-技~ 22861            98.83            ""              
    ##  6 2014   營造業-服務及~ 25075            99.74            23125           
    ##  7 2014   其他服務業-專~ 29500            99.66            ""              
    ##  8 2014   資訊及通訊傳播~ 30686            99.2             ""              
    ##  9 2014   不動產業-專業~ 32126            99.84            ""              
    ## 10 2014   教育服務業-事~ 22325            95.75            ""              
    ## # ... with 23 more variables: `國中及以下-女/男.x` <chr>,
    ## #   `高中或高職-薪資.x` <chr>, `高中或高職-女/男.x` <chr>,
    ## #   `專科-薪資.x` <chr>, `專科-女/男.x` <chr>, `大學-薪資.x` <chr>,
    ## #   `大學-女/男.x` <chr>, `研究所及以上-薪資.x` <chr>,
    ## #   `研究所及以上-女/男.x` <chr>, 年度.y <chr>, `經常性薪資-薪資.y` <chr>,
    ## #   `經常性薪資-女/男.y` <chr>, `國中及以下-薪資.y` <chr>,
    ## #   `國中及以下-女/男.y` <chr>, `高中或高職-薪資.y` <chr>,
    ## #   `高中或高職-女/男.y` <chr>, `專科-薪資.y` <chr>, `專科-女/男.y` <chr>,
    ## #   `大學-薪資.y` <chr>, `大學-女/男.y` <chr>,
    ## #   `研究所及以上-薪資.y` <chr>, `研究所及以上-女/男.y` <chr>,
    ## #   薪資比 <dbl>

將106年大學畢業薪資除以103年大學畢業薪資算出薪資比，由此可知大於1的值就是106年薪資大於103年薪資的值，比例越大差異越大。
\#\#\# 提高超過5%的的職業有哪些?

``` r
#這是R Code Chunk
pp2<-payjoin[payjoin$薪資比>1.05,]
pp2
```

    ## # A tibble: 58 x 28
    ##    年度.x 大職業別 `經常性薪資-薪資.x`~ `經常性薪資-女/男.x`~ `國中及以下-薪資.x`~
    ##    <chr>  <chr>    <chr>            <chr>            <chr>           
    ##  1 2014   其他服務業-技~ 24724            89.1             ""              
    ##  2 2014   住宿及餐飲業-~ 21647            98.45            20992           
    ##  3 2014   用水供應及污染~ 28552            99.46            ""              
    ##  4 2014   專業科學及技術~ 32078            99.63            ""              
    ##  5 2014   其他服務業-技~ 22861            98.83            ""              
    ##  6 2014   營造業-服務及~ 25075            99.74            23125           
    ##  7 2014   其他服務業-專~ 29500            99.66            ""              
    ##  8 2014   資訊及通訊傳播~ 30686            99.2             ""              
    ##  9 2014   不動產業-專業~ 32126            99.84            ""              
    ## 10 2014   教育服務業-事~ 22325            95.75            ""              
    ## # ... with 48 more rows, and 23 more variables:
    ## #   `國中及以下-女/男.x` <chr>, `高中或高職-薪資.x` <chr>,
    ## #   `高中或高職-女/男.x` <chr>, `專科-薪資.x` <chr>, `專科-女/男.x` <chr>,
    ## #   `大學-薪資.x` <chr>, `大學-女/男.x` <chr>,
    ## #   `研究所及以上-薪資.x` <chr>, `研究所及以上-女/男.x` <chr>,
    ## #   年度.y <chr>, `經常性薪資-薪資.y` <chr>, `經常性薪資-女/男.y` <chr>,
    ## #   `國中及以下-薪資.y` <chr>, `國中及以下-女/男.y` <chr>,
    ## #   `高中或高職-薪資.y` <chr>, `高中或高職-女/男.y` <chr>,
    ## #   `專科-薪資.y` <chr>, `專科-女/男.y` <chr>, `大學-薪資.y` <chr>,
    ## #   `大學-女/男.y` <chr>, `研究所及以上-薪資.y` <chr>,
    ## #   `研究所及以上-女/男.y` <chr>, 薪資比 <dbl>

### 主要的職業種別是哪些種類呢?

``` r
#這是R Code Chunk
career<-strsplit(pp2$大職業別,"-")
career<-sapply(career,"[",1)
table(career)
```

    ## career
    ##     工業及服務業部門             工業部門             不動產業 
    ##                    1                    1                    1 
    ##           支援服務業 用水供應及污染整治業         住宿及餐飲業 
    ##                    3                    6                    4 
    ##           其他服務業           服務業部門         金融及保險業 
    ##                    5                    5                    1 
    ## 專業科學及技術服務業           教育服務業     資訊及通訊傳播業 
    ##                    5                    5                    5 
    ##         運輸及倉儲業     電力及燃氣供應業               製造業 
    ##                    4                    2                    1 
    ##               營造業       醫療保健服務業 藝術娛樂及休閒服務業 
    ##                    3                    2                    3 
    ##     礦業及土石採取業 
    ##                    1

## 男女同工不同酬現況分析

男女同工不同酬一直是性別平等中很重要的問題，分析資料來源為103到106年度的大學畢業薪資。

### 103到106年度的大學畢業薪資資料，哪些行業男生薪資比女生薪資多?

``` r
#這是R Code Chunk
pay103$`大學-女/男`<-as.numeric(pay103$`大學-女/男`)
pay103b<-pay103[pay103$`大學-女/男`<100,]%>%
  na.omit(pay103b)%>%
  arrange((`大學-女/男`))%>%
  head(10)
pay104$`大學-女/男`<-as.numeric(pay104$`大學-女/男`)
pay104b<-pay104[pay104$`大學-女/男`<100,]%>%
  na.omit(pay104b)%>%
  arrange((`大學-女/男`))%>%
  head(10)
pay105$`大學-女/男`<-as.numeric(pay105$`大學-女/男`)
pay105b<-pay105[pay105$`大學-女/男`<100,]%>%
  na.omit(pay105b)%>%
  arrange((`大學-女/男`))%>%
  head(10)
pay106$`大學-女/男`<-as.numeric(pay106$`大學-女/男`)
pay106b<-pay106[pay106$`大學-女/男`<100,]%>%
  na.omit(pay106b)%>%
  arrange((`大學-女/男`))%>%
  head(10)

a<-data.frame(top103=head(pay103b$大職業別,10),
              top104=head(pay104b$大職業別,10),
              top105=head(pay105b$大職業別,10),
              top106=head(pay106b$大職業別,10))
a
```

    ##                                             top103
    ## 1      礦業及土石採取業-技藝機械設備操作及組裝人員
    ## 2            教育服務業-技藝機械設備操作及組裝人員
    ## 3                  其他服務業-技術員及助理專業人員
    ## 4      電力及燃氣供應業-技藝機械設備操作及組裝人員
    ## 5              礦業及土石採取業-服務及銷售工作人員
    ## 6                                           營造業
    ## 7                          教育服務業-事務支援人員
    ## 8                                       教育服務業
    ## 9  藝術娛樂及休閒服務業-技藝機械設備操作及組裝人員
    ## 10                                      其他服務業
    ##                                             top104
    ## 1      電力及燃氣供應業-技藝機械設備操作及組裝人員
    ## 2                    教育服務業-服務及銷售工作人員
    ## 3            礦業及土石採取業-技術員及助理專業人員
    ## 4      礦業及土石採取業-技藝機械設備操作及組裝人員
    ## 5                                 礦業及土石採取業
    ## 6                          其他服務業-事務支援人員
    ## 7                營造業-技藝機械設備操作及組裝人員
    ## 8  用水供應及污染整治業-技藝機械設備操作及組裝人員
    ## 9                                           營造業
    ## 10                                      教育服務業
    ##                                         top105
    ## 1          不動產業-技藝機械設備操作及組裝人員
    ## 2                      醫療保健服務業-專業人員
    ## 3            用水供應及污染整治業-事務支援人員
    ## 4                          營造業-事務支援人員
    ## 5                        不動產業-事務支援人員
    ## 6                                       營造業
    ## 7                              營造業-專業人員
    ## 8  資訊及通訊傳播業-技藝機械設備操作及組裝人員
    ## 9                  不動產業-服務及銷售工作人員
    ## 10                                  其他服務業
    ##                                         top106
    ## 1  電力及燃氣供應業-技藝機械設備操作及組裝人員
    ## 2                    營造業-服務及銷售工作人員
    ## 3                      其他服務業-事務支援人員
    ## 4        電力及燃氣供應業-技術員及助理專業人員
    ## 5                                   其他服務業
    ## 6      住宿及餐飲業-技藝機械設備操作及組裝人員
    ## 7                                       營造業
    ## 8                          教育服務業-專業人員
    ## 9                    運輸及倉儲業-事務支援人員
    ## 10             其他服務業-技術員及助理專業人員

### 哪些行業女生薪資比男生薪資多?

``` r
#這是R Code Chunk
pay103c<-pay103[pay103$`大學-女/男`>100,]%>%
  arrange(desc(`大學-女/男`))%>%
  head(10)
pay104c<-pay104[pay104$`大學-女/男`>100,]%>%
  arrange(desc(`大學-女/男`))%>%
  head(10)
pay105c<-pay105[pay105$`大學-女/男`>100,]%>%
  arrange(desc(`大學-女/男`))%>%
  head(10)
pay106c<-pay106[pay106$`大學-女/男`>100,]%>%
  arrange(desc(`大學-女/男`))%>%
  head(10)

b<-data.frame(top103=head(pay103c$大職業別,10),
              top104=head(pay104c$大職業別,10),
              top105=head(pay105c$大職業別,10),
              top106=head(pay106c$大職業別,10))
b
```

    ##    top103                                          top104
    ## 1    <NA> 專業科學及技術服務業-技藝機械設備操作及組裝人員
    ## 2    <NA>                                            <NA>
    ## 3    <NA>                                            <NA>
    ## 4    <NA>                                            <NA>
    ## 5    <NA>                                            <NA>
    ## 6    <NA>                                            <NA>
    ## 7    <NA>                                            <NA>
    ## 8    <NA>                                            <NA>
    ## 9    <NA>                                            <NA>
    ## 10   <NA>                                            <NA>
    ##                   top105                              top106
    ## 1  金融及保險業-專業人員 資訊及通訊傳播業-服務及銷售工作人員
    ## 2                   <NA>                                <NA>
    ## 3                   <NA>                                <NA>
    ## 4                   <NA>                                <NA>
    ## 5                   <NA>                                <NA>
    ## 6                   <NA>                                <NA>
    ## 7                   <NA>                                <NA>
    ## 8                   <NA>                                <NA>
    ## 9                   <NA>                                <NA>
    ## 10                  <NA>                                <NA>

將103到106年的大學男女畢業薪資比取出來由大到小排序，大於100的值代表女生畢業薪資大於男性。 \#\# 研究所薪資差異

以106年度的資料來看，哪個職業別念研究所最划算呢 (研究所學歷薪資與大學學歷薪資增加比例最多)?

``` r
#這是R Code Chunk
pay106d<-pay106[,c(2,11,13)]
pay106d$薪資比<-as.numeric(pay106d$`研究所及以上-薪資`)/as.numeric(pay106d$`大學-薪資`)
pay106d<-na.omit(pay106d)%>%
  arrange(desc(薪資比))
head(pay106d,10)
```

    ## # A tibble: 10 x 4
    ##    大職業別                          `大學-薪資` `研究所及以上-薪資` 薪資比
    ##    <chr>                             <chr>       <chr>                <dbl>
    ##  1 礦業及土石採取業-事務支援人員     24815       30000                 1.21
    ##  2 專業科學及技術服務業              29648       35666                 1.20
    ##  3 其他服務業-技術員及助理專業人員   27929       33500                 1.20
    ##  4 專業科學及技術服務業-事務支援人員 27035       32234                 1.19
    ##  5 批發及零售業                      27611       32910                 1.19
    ##  6 製造業                            28155       33458                 1.19
    ##  7 藝術娛樂及休閒服務業-事務支援人員 24970       29657                 1.19
    ##  8 工業部門                          28263       33448                 1.18
    ##  9 工業及服務業部門                  28446       33633                 1.18
    ## 10 服務業部門                        28715       33922                 1.18

將研究所畢業薪資除已大學畢業薪資的到的薪資比表示其增加的比例，比例越高起薪差距越大。 \#\# 我有興趣的職業別薪資狀況分析

### 有興趣的職業別篩選，呈現薪資

``` r
#這是R Code Chunk
f<-pay106d[30,]
f
```

    ## # A tibble: 1 x 4
    ##   大職業別 `大學-薪資` `研究所及以上-薪資` 薪資比
    ##   <chr>    <chr>       <chr>                <dbl>
    ## 1 營造業   28952       33210                 1.15

### 這些職業別研究所薪資與大學薪資差多少呢？

``` r
#這是R Code Chunk
as.numeric(f$`研究所及以上-薪資`)-as.numeric(f$`大學-薪資`)
```

    ## [1] 4258

結果得知研究所起薪比大學高4000多，跟我原先的認知差不多，所以不會影響我的決定，我還是不會讀研究所。
