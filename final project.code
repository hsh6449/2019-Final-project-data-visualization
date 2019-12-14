library(rvest)
library(httr)
library(RSelenium)

#Rselenium 실행 java -Dwebdriver.gecko.driver="geckodriver.exe" -jar selenium-server-standalone-3.9.1.jar -port 4446
remDr <- remoteDriver(
  remoteServerAddr = "localhost",
  port = 4446L,
  browserName = "chrome"
)

remDr$open()
remDr$navigate("https://kr.trip.com/")

#사이트내에서 호텔탭으로 처음나와있어서 항공권 탭을 클릭
tab_flight <- remDr$findElement(using = "xpath",value = '//*[@id="main-search-box"]/div[2]/div[1]/ul/li[2]/span')
tab_flight$clickElement()

#노선을 선택하는 과정
tab_Dcity <- remDr$findElement(using = "xpath",value = '//*[@id="msf-body"]/div[1]/div[1]/div[1]/div/label')
tab_Dcity$clickElement()
Dcity <-remDr$findElement(using="xpath",value='//*[@id="address_suggestionContainer_msf-sin-depart-city-input"]/div/div[1]/ul/li/a[1]')
Dcity$clickElement()#출발도시 탭을 누르고 출발도시 설정
tab_Acity <- remDr$findElement(using = 'xpath',value='//*[@id="msf-body"]/div[1]/div[1]/div[3]/div/label')
tab_Acity$clickElement()
Acity <-remDr$findElement(using="xpath",value='//*[@id="address_suggestionContainer_msf-sin-arrival-city-input"]/div/div[2]/ul/li/a[4]')
Acity$clickElement()#도착도시 탭을 누르고 도착도시 설정

tab_Depature <- remDr$findElement(using = 'xpath',value = '//*[@id="msf-body"]/div[1]/div[2]/div[1]/div/label')
tab_Depature$clickElement()#여행시작일
nextmonth <- remDr$findElement(using = "xpath",value = '//*[@id="next-month"]/i')
nextmonth$clickElement()#여행시작일 탭에서 다음달 선택하기
date_Dcity <- remDr$findElement(using = "xpath",value = '//*[@id="c_calendaruid_15762595424536860268845"]/div/table/tbody/tr[4]/td[2]')
date_Dcity$clickElement()#여행시작일 선택(2월17일)
date_Acity <- remDr$findElement(using = "xpath",value = '//*[@id="c_calendaruid_15762595424555852607172"]/div/table/tbody/tr[4]/td[7]')
date_Acity$clickElement()#여행종료일 선택(2월22일)
search_btn<-remDr$findElement(using="xpath",value='//*[@id="searchBtn"]')
search_btn$clickElement()#확인버튼

rowsort <- remDr$findElement(using = "xpath",value='//*[@id="main"]/div/div[2]/div[4]/div[1]/div[1]/div/div[1]/div[2]/div/div[1]/div/p[1]')
rowsort$clickElement()#가격내림차순으로 정렬

#필요한 정보만 선별해서 따온다 항공사,가격,출발시간,도착시간
airline_name <- remDr$findElement(using = "xpath",value='//*[@id="J_resultList"]/div[*]/div/div/div[1]/div[1]/div/div[1]')
price <- remDr$findElement(using = "xpath",value='//*[@id="J_resultList"]/div[*]/div/div/div[1]/div[4]/div/div[1]/div/span/em')
D_time <- remDr$findElement(using = "xpath",value='//*[@id="J_resultList"]/div[*]/div/div/div[1]/div[2]/div[1]/div/div[1]')
A_time <- remDr$findElement(using = "xpath",value = '//*[@id="J_resultList"]/div[*]/div/div/div[1]/div[2]/div[3]/div/div[1]')


#텍스트로변환
page_source <- remDr$getPageSource()[[1]]
aline_name <-read_html(page_source) %>%
  html_nodes(xpath='//*[@id="J_resultList"]/div[*]/div/div/div[1]/div[1]/div/div[1]')%>%
  html_text()
price <-read_html(page_source) %>%
  html_nodes(xpath='//*[@id="J_resultList"]/div[*]/div/div/div[1]/div[4]/div/div[1]/div/span/em')%>%
  html_text()
Dtime <-read_html(page_source) %>%
  html_nodes(xpath='//*[@id="J_resultList"]/div[*]/div/div/div[1]/div[2]/div[1]/div/div[1]')%>%
  html_text()
Atime <-read_html(page_source) %>%
  html_nodes(xpath='//*[@id="J_resultList"]/div[*]/div/div/div[1]/div[2]/div[3]/div/div[1]')%>%
  html_text()
flighthour <-read_html(page_source) %>%
  html_nodes(xpath='//*[@id="J_resultList"]/div[*]/div/div/div[1]/div[3]/div[1]')%>%
  html_text()

price <-  gsub(',',"",price)

flight <- list(aline_name,Dtime,Atime,price,flighthour)
names(flight) <- c('Flight','Dtime','Atime','price','flighthour')

flight
#데이터프레임으로 만들기위해 결측치 해결
F.flight$Flight <- trimws(x = F.flight$Flight,
       which = "right")
F.flight$Flight[8] <- "대한항공"
flight$Dtime[59] <- '12:00'
flight$Atime[59] <- '14:00'
flight$price[59] <- '250000' #중앙값으로 처리
flight$flighthour[59] <- '2'
F.flight <- as.data.frame(flight)

#간단한 통계처리,가격들의 평균,평균 걸리는 시간
Price.N <- gsub(',',"",price)#숫자형식으로 바꿔주기위해 반점 삭제
mean(as.numeric(Price))#평균은 287308.6
median(as.numeric(sPrice))#중앙값은 228500

#소요시간 중앙값
flighthour <- gsub('시간','.',flighthour)
flighthour <- gsub('분','',flighthour)#숫자로 나타내기위해 글자를 처리함
median(as.numeric(flighthour))#2.23(2시간 23분)
mean(as.numeric(flighthour))#3.42(3시간 42분)


#스케줄러만들기위해 폴더 생성
folder <- 'C:/schedule'
if(!dir.exists(folder)) dir.create(folder)

setwd(folder)
write.table(F.flight,'F_flight.txt')

setwd(folder)

require(lubridate)

date <- Sys.Date()
h <- hour(Sys.time())
m <- minute(Sys.time())
now <- paste(date, h, m, sep='-')
now.folder <- paste(folder, now, sep='/')

url = 'https://kr.trip.com/flights/seoul-to-shanghai/tickets-sel-sha?dcity=sel&acity=sha&ddate=2020-02-17&rdate=2020-02-22&flighttype=rt&class=ys&quantity=1&nonstoponly=off&searchboxarg=t'
for(i in 1){
  file <- read_html(url)

  # 웹 사이트에서 다운받은 페이지 저장
  write_xml(file, file = file.name[i])
}

# 폴더에 저장된 웹 페이지에서 데이터를 얻는다.
extract = function(file) {
  html <- read_html(file)
  table <- html %>% html_nodes("table")
  td <- table %>% html_nodes("td")
  text <- td %>% html_text()
  text <- gsub("(\r)(\n)(\t)*", "", text)
  df <- as.data.frame(matrix(text, nrow=10, ncol=6, byrow=TRUE))

  link <- html %>% html_nodes("a.hover-link")
  dataIdx <- gsub("<a.*dataIdx=|&.*", "", link)
  dataIdx <- dataIdx[c(TRUE, FALSE)]
  df <- cbind(df, dataIdx)
}

result <- lapply(file.name, extract)
do.call(rbind, result)
#정기적으로 R실행하기 위해 텔레그램 봇 이용


#시각화를 위해서 필요한 패키지 부착
library(tidyverse)
library(KoNLP)
library(wordcloud)

useSejongDic()
data1 <- readLines('F_flight.txt')

#전처리 과정
data2 <- sapply(data1,extractNoun,USE.NAMES=F)
data3 <- unlist(data2)
data3 <- Filter(function(x){nchar(x)>=2},data3)

data3 <- gsub('\\d','',data3)
data3 <- gsub(' ','',data3)
data3 <- gsub('_','',data3)
data3 <- gsub(',','',data3)


write(unlist(data3),'dda1.txt')
data4 <- read.table('dda1.txt')

wordcount <- table(data4)
wordcount <- head(sort(wordcount,decreasing = T),60)

#wordcloud패키지를 이용한 시각화
library(RColorBrewer)
palete <- brewer.pal(9,"Set1")
wordcloud(names(wordcount),freq = wordcount,scale=c(5,0.5),rot.per = 0.25,min.freq = 1,
          random.order = F,random.color = T,colors = palete)
legend(0.3,1,'상하이행 비행기 항공사 목록',cex=1,fill=NA,bg='white',
       text.col='black',text.font=2,box.col='black')

#가격그래프
install.packages("ggrepel")
library("ggrepel")

#항공사별 가격
ggplot(F.flight,aes(x=price,y=flighthour))+
  geom_point(aes(color=Flight))+
  geom_text_repel(aes(label=Flight),size=2)+
  coord_fixed()+
  scale_x_continuous(breaks = seq(0,600000,100000))

#가격별 소요시간
ggplot(F.flight,aes(x=price,y=flighthour))+
  geom_point(aes(color=Flight))+
  geom_line(aes(y = flighthour))
