# hunet-project
휴넷에서 진행한 k-digital training에서 탤런트 뱅크(인재매칭 기업)을 대상으로 진행한 프로젝트

이력서-공고 매칭 시스템 - text classification과 Doc2Vec을 이용하여 기업에서 가지고 있는 이력서와 가장 유사한 공고를 찾아주는 시스템 개발

![image](https://user-images.githubusercontent.com/93864811/213355121-a3fdc188-6dfd-469d-a958-3f5abb8d4fc2.png)

# 환경
google colab
jupyter notebook

# member
* 손창진 - 모델설계, oversmapling, 분류 성능 증가시키기
* 전창석 - crawling, doc2vec
* 강기훈 - crawling, 분류 성능 증가시키기

# Data
사람인 헤드헌팅 채용공고에서 크롤링하여 사용

![image](https://user-images.githubusercontent.com/93864811/213355293-73dffdf5-195a-4bb3-832f-dd0b13b95ead.png)

# 직무분야 분류
* 처음에는 이력서와 공고를 바로 유사도를 계산하여 관련도가 가장 높은 공고를 추천하도록 할려고 하였으나 정확도가 떨어짐 
(헤드헌팅 공고 특성상 공고 초반부분에 기업설명이 있는 경우가 많아 이 부분에서 문제가 발생)
*  ex) it기업에서 hr직무를 채용하는 경우 it 관련 공고를 추천
*  공고 추천의 정확성을 좀 더 높이기 위해 1차적으로 이력서의 직무분야를 분류 후 그 카테고리에 해당하는 공고들 중 추천하는 경우로 바꿈

### 직무 분야별 데이터 class imbalance
![image](https://user-images.githubusercontent.com/93864811/213357099-5dceba95-f79f-4b91-820f-5c7daed71595.png)

* 크롤링 당시 가장 많은 공고의 경우 2000개를 넘고 가장 적은 공고의 경우 1개로 class imbalance가 심각한 상태
* 비슷한 분야끼리는 합치고 수가 적은 공고는 oversampling을 통하여 불균형을 어느정도 해결(공고의 수가 너무 작은 경우는 삭제)
* 총 5개의 class로 분류
* Shuffle Oversampling 

![image](https://user-images.githubusercontent.com/93864811/213358932-22e21b0b-2652-491b-8b37-63d0015c2517.png)

| Oversampling Method  | f1-score |
| ------------- | ------------- |
| No Oversampling  |0.5975536099847311|
| Random Oversampling  |0.7643383007079005|
| Shuffle Oversampling  |0.7484113390543973|

=> Random Oversampling이 f1-score측면에서는 조금 더 높았지만 혼동행렬을 봤을 때 Shuffle Oversampling이 class별로 좀 더 고른 분포를 보여 Shuffle 하는 방법을 사용

# Doc2Vec
* Word2Vec의 확장된 개념으로 문서나 문장단위로 임베딩을 해주는 모델
* 타겟 단어와 이전 단어 k 개가 주어졌을 때, 이전 단어들 + 해당 문서의 아이디로 타겟 단어를 예측하는 과정에서 문맥이 비슷한 문서 벡터와 단어 벡터가 유사하게(코사인 유사도) 임베딩됨
* PV-DM과 PV-DBOW 중 PV-DM이 성능이 더 좋게 나오는 것 같아 PV-DM으로 사용
* PV-DM 

![image](https://user-images.githubusercontent.com/93864811/213360671-1f4314f4-3aa3-4b3e-a593-2584d6f6cf40.png)

* PV-DBOW

![image](https://user-images.githubusercontent.com/93864811/213360729-94358d49-afb2-49db-adb3-7654dc61550d.png)

# 유사도기반 공고 추천
* Doc2Vec을 통해 얻어진 문서벡터로 cosine similarity를 계산하여 가장 유사한 공고 top 5개를 보여줌

# Reuslts

* resume1 - 마케팅(디지털, 온라인, 매체 관련 마케팅 경력)
![image](https://user-images.githubusercontent.com/93864811/218108967-3095aedb-088d-4755-a58b-44c277593367.png)

![image](https://user-images.githubusercontent.com/93864811/213361886-332cd8ef-06fc-433f-aa36-315cb617d64a.png)

* resume2 - 마케팅(브랜딩 관련 경력)
![image](https://user-images.githubusercontent.com/93864811/218109153-08dccce9-eea7-4d41-8e48-5f8fa0f32672.png)

![image](https://user-images.githubusercontent.com/93864811/213362096-0f2733c0-71de-4e6e-ac17-6c2d1cb6cf20.png)







