# README

# Text-Mining Project

## ****📅 프로젝트 기간****

1. 07. 13 ~ 22. 08. 04

## ****📔 프로젝트 목적****

텍스트 마이닝을 이용해서 네이버 맛집 블로그 광고글 구분하기

## ****💪역할****

- 강동호 :
- 여민희 :
- 유한솔 :
- 윤병윤 :
- 이정우 :
- 황성연 :

## ****🗄️ 데이터셋****

- 네이버 맛집 블로그 크롤링
    - 데이터 셋 총 ~~ 개

## ****⚙️ Process****

- 광고글과 비광고글 경향 파악
    - [실제 업체에서 요구하는 양식](https://www.revu.net/)  + [빅데이터 기반 단어 분석 채널](https://some.co.kr/) 이용
    - ‘소정의 원고료를 받아 작성한 글입니다’가 포함된 글 분석
- 광고글 패턴 분석 결과
    - 본문에 ‘소정의’, ‘원고료’, ‘문의’, ‘업체’, ‘체험단’, ‘협찬’ 단어가 사용됨
    - 대부분 15장 이상의 이미지가 사용되고, 본문의 길이가 800자 이상
    - 상위 노출을 위해 제목의 키워드를 5회 이상 반복적으로 사용
    - 지도정보와 비디오를 첨부하고 있음
    - 본문에 식당의 영업에 대한 자세한 정보(영업시간, 오시는 길, 전화번호, 브레이크타임)가 표기됨
    - 스티커가 사용되는 광고글의 경우 최소 5개 이상의 스티커가 사용됨
    - 인스타, 블로그 댓글 등을 통해 식당에 관한 문의를 달라고 요청함
    - 비싸다, 짜다, 불친절 등의 부정적인 단어들은 잘 사용되지 않음
- 데이터 수집 - BeautifulSoup, request 사용
- 크롤링 과정
    - ㅇ
- 크롤링 결과
    - 블로그 제목(Title), 본문 내용(Body), 이미지 수(Img), 스티커 수(Sticker), 지도 유무(Map), 비디오 유무(Video), 본문 길이(Body_len), 광고/비광고(Label)
        
        ![Untitled](README%20bac759cffe5a4c818716ce14ff9253de/Untitled.png)
        
- 전처리
    - 한글 및 공백을 제외한 문자 제거
    - Okt 객체를 활용해서 형태소 토큰화 + 품사 태깅
    - 불용어 제거
    - 명사, 동사, 형용사 추출
- 벡터화
    - TF-IDF Vectorizer 이용
- 모델링
    - Logistic Regression
    - RandomForestClassifier
    - ExtraTressClassifier
    - AdaBoost Classifier
    

****📝 Machine Learning****

- 예측과정
    - body(텍스트 마이닝을 통해 라벨 예측) + Img, Sticker, Map, Video, Body_len(표준화 작업 후 라벨 예측) → voting Classifier
    - 2가지 경우로 학습
        - 본문 내용을 제외한 특성으로 학습
        - 본문 내용만을 이용해서 학습
            - 토큰을 직접 만들어줌 → 본문 하단에 직접 추가
                - 이미지 > 25 : 낟쭘
                - 15 < 이미지 < 25 : 덩셤
                - 8 < 이미지 < 15 : 횡뤼
                - 비디오 > 0 : 굇묜
                - 식당이름 본문 언급횟수 > 4 : 뉠튠
                - 지도 > 0 : 썅꽜
                - 스티커 > 5 : 춥륄
                - 본문길이 < 800 : 스탤
            - 불용어 제거 작업
                - 광고/비광고 구분에 필요 없는 단어를 불용어 사전에 추가, 이를 이용해 불용어 단어 제거(단, 직접 만들어준 토큰 제외)
- 데이터 전처리
    - 지역별 10개의 맛집 후기 csv로 추출 후 하나로 합침
    - NULL값 제거
    - 본문내용(Body) 기준 중복 제거
    - 표준화 작업(StandardScaler)
- GridSearchCV사용하여 최적의 하이퍼파라미터 값을 찾음
    - 본문을 제외한 특성을 사용한 머신러닝 학습
    - GridSearchCV 미사용
        
        
        | Model | Test_score |
        | --- | --- |
        | RandomForestClassifier | 0.743 |
    - GridSearchCV 사용 - 성능 향상 확인
        
        
        | Model | Test_score |
        | --- | --- |
        | RandomForestClassifier | 0.784 |
- 사용한 모델
    - Logistic Regression
    - RandomForestClassifier
    - ExtraTreesClassifier
    - AdaBoostClassifier

### **📊 결과**

- 단일 모델 결과
    
    
    | Model | 본문 이외 특성 Test_score | 본문 특성 Test_score |
    | --- | --- | --- |
    | LogisticRegression | 0.68 | 0.74 |
    | RandomForestClassifier | 0.78 | 0.71 |
    | ExtraTreesClassifier | 0.72 | 0.74 |
    | AdaBoostClassifier |  | 0.74 |

 

- Ensemble 결과
    
    
    | Ensemble | Test_score |
    | --- | --- |
    | Soft Voting(본문만 모델 4종) | 0.74 |
    | Hard voting(본문만 모델 4종) | 0.73 |
    | Soft Voting(본문 + 본문 이외 특성) | 0.71 |
    | Hard Voting(본문 + 본문 이외 특성) | 0.70 |

### 🔥 한계점

- 라벨(광고/비광고성)에 대한 주관성
- 네이버 블로그의 높은 정보 오염도
- 카운트 기반의 텍스트마이닝의 한계 → 딥러닝의 필요성

### 🙌🏻 보완점

- Feature importance를 찍어서 각 feature의 중요성을 확인해보기
- 불용어 제거 전, 후의 성능 차이 확인