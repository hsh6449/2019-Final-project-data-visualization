데이터 시각화 Final-project
==========================
- 주제 : 서울에서 상하이 가는 여행을 계획하기위해 항공편 정보 크롤링
-------------------------------
> # 1. 개요
 19년도 2학기 데이터시각화 수업에서 한 학기동안 배운 것을 이용하여 Final project를 진행하였다.
 미흡한 실력이지만 한 학기 동안 배운 것을 총 동원해서 진행했다. Final project 주제로 서울-상하이 편 항공권에 대한 정보를 '트립닷컴'이라는 사이트에서 크롤링 했다.
 원래 '스카이스캐너'라는 사이트에서 크롤링을 진행하려고 했지만 웹크롤링을 서버자체로 막아놨기 때문에 대안으로 '트립닷컴'을 선택했다.
 
> # 2. 크롤링한 데이터가 유용한 이유
* 최근 항공권 비교사이트들이 많아 항공가격을 비교하기 쉬워지긴 했으나 매일 변하는 가격으로 인해 언제, 얼마에 살지 결정하기 어렵다.
* 항공권은 실시간으로 가격이 바뀌므로 사이트의 많은 정보보다 필요한 정보만을 뽑아서 비교해보는 것이 더 유용해 보였다.
* 크롤링한 데이터 (가격,출발시간,도착시간,소요시간, 항공사)를 토대로 가격차이마다, 소요시간 차이마다 유의미한 관련이 있는지 알아 볼 수 있다.
* 따라서 얼마에 항공권을 살 것인지에 대한 *가이드라인* 이 될 수 있다.

> # 3. 과정
1. cmd를 킨 다음 Rselenium을 실행했다.
2. R studio에서 Rselenium 패키지를 사용해서 트립닷컴에 들어가서 크롤링을 진행했다.
![r1](https://user-images.githubusercontent.com/57973170/70851997-8c621580-1edf-11ea-9c19-6d5078130eaa.jpg)
![r2](https://user-images.githubusercontent.com/57973170/70852024-e236bd80-1edf-11ea-8a23-5fa04d389960.jpg)
* 동그라미 친 부분만 크롤링 했다.
3. 크롤링 한 후 데이터 전 처리 (필요없는 문자들 삭제)
![r3](https://user-images.githubusercontent.com/57973170/70852053-3e014680-1ee0-11ea-8115-57124186556f.jpg)
4. 통계처리
![r4](https://user-images.githubusercontent.com/57973170/70852080-9df7ed00-1ee0-11ea-812c-463cada94e69.jpg)
-즉, 약 **22만원** 이하, 약 **2시간반** 이하의 비행시간의 항공권은 매우 합리적이라고 판단할 수 있다. 
5. 매일매일 항공가격이 변하므로 telegrambot을 이용한 스케줄러 등록
<img width="692" alt="스케줄러1" src="https://user-images.githubusercontent.com/57973170/70852129-2c6c6e80-1ee1-11ea-878d-39127e0fd3dc.png">
<img width="413" alt="스케줄러2" src="https://user-images.githubusercontent.com/57973170/70852130-2d050500-1ee1-11ea-8f6b-e8d8698639fd.png">
6. 시각화 처리
<img width="473" alt="노선 " src="https://user-images.githubusercontent.com/57973170/70852139-44dc8900-1ee1-11ea-99bb-6d4d18237076.png">

* wordcloud를 이용한 그림
* 한국과 중국 항공사로 이루어져있음
* 대한항공의 노선이 가장 많은 것으로 나타난다.

<img width="473" alt="가격대비 소요 시간 " src="https://user-images.githubusercontent.com/57973170/70852160-80775300-1ee1-11ea-920d-5092fb72f046.png">

* 가격이 비싸다고 해서 빨리 도착하는 것만은 아니다. 
* 싸고 빨리 도착하는 항공편도 있다.
* 소요시간만이 가격에 영향을 미치는 것은 아닌듯 하다.

<img width="550" alt="진짜price " src="https://user-images.githubusercontent.com/57973170/70852178-dcda7280-1ee1-11ea-8cd4-63605a366e6f.png">

* 중국항공사가 싼 편에 속하고 아무래도 바로 중국으로 가다보니까 소요시간 역시 얼마 안걸리는 것으로 나타났다.

> # 4. 결론
* 22만원 이하의 항공권중 소요시간이 2시간정도 걸리는 항공권을 산다면 잘 산 것이다. 그러므로 스케줄러를 잘 확인해서 여행계획에 참고하도록 하자
* 우리나라의 대한항공이 신뢰도 높고 노선 또한 다양하지만 , 저렴하고 빠른 것은 아무래도 중국항공사이기때문에 본인의 가치관에 따라 선택할 수 있다. 

> # 5. 마치며

* 많은 시행착오가 있었지만 원하는 결과를 어느정도 얻어낸 것 같다.
* R을 처음 다뤄봤기 때문에 도중에 포기하고 싶은 순간이 많았지만 이렇게 결과물을 조금이나마 낼 수 있게 되어서 자랑스럽다.
* 이번 프로젝트도 시각화처리를 비롯해서 미흡한 부분이 상당히 많지만 학기 시작 전 아무것도 할 줄 모르는 모습과 비교했을때 이정도도 큰 성과라고 생각하기에 후련한 마음이 든다.
* 그 동안은 기존 코드에서 몇가지만 바꾸는 수준이었다면, 이번 프로젝트는 오로지 배운 것과 구글링을 한 것을 이용해 스스로 코드를 짜서 진행해서 큰 성취감을 느꼈다.
> # 
