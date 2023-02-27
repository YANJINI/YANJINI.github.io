---
title: Credit Risk Modeling using Home Credit Dataset
subtitle: Using Python, I build and evaluate a logistic regression model under Naive Bayes assumption with Weight of Evidence transformed features to predict the probability of default for 307,511 loans in the Kaggle Home Credit dataset.
layout: default
date: 2023-02-15
keywords: blogging, writing, project
published: false
categories: [all, crm]
---
저번학기 credit scoring 수업이야기. 교수는 매우 열정충만하고 좋았지만 프로젝트가 그냥 자기가 만든 파일 그대로 유지하면서 컷오프만 바꿔 수익을 늘리는 거라 수업 배운 내용을 전혀 써먹을 수 없었다. 그래서 udemy의 수업(링크걸W)을 들은 뒤에, 또 다른 데이터인 homecredit으로 배운 내용은 다시 적용한 뒤 깔끔하게 documentation하는 것이 목표.
[Home Credit Default Risk](https://www.kaggle.com/competitions/home-credit-default-risk/data)
왜 데이터는 8개 csv인데 그중에서 train, test만 쓰려고 하는지를 다른 데이터셋의 특성과 함께 정리해보기 ()


## 전체적인 pd model 흐름도
전체적으로 어떻게 모델링이 진행되는지 설명  (udemy crm 수업 참조)
- EDA를 통해 features와 target에 대한 이해를 높히고 missing value나 바꿔야할 value의 경우 (데이터타입에 문제있는 경우)를 파악해두기.

- 위에서 발견한 문제들을 반영해서 조금씩 수정해주 (na impute를 하거나 등등)

- Weight of Evidence으로 변형하는 이유를 logistic regression이랑 엮어서 수식적으로 설명하고, Naive Bayes assumption을 은연 중에 가정하고 있음을 언급하기. 이와 동시에 Naive Bayes의 특수한 경우가 logistic regression과 접점이 있음을 알리기 (코넬 수업 강의자료 링크걸기), WoE 변환이 이루어진 대표적인 feature 하나를 그래프와 함께 소개하면서 예시를 들, 부록에다가 feature를 WoE화하는 파이썬 함수 첨부하기. + IV 점수로 pre selection해서 떨궈진 변수들 설명

- 일종의 time-series인 behavioral data csv로 새로운 feature 만들어볼 수 있을 듯. (exponential decay를 활용해서 최근의 정보의 큰 weight를 반영하는 식으로)

- logistic regression 모델을 설명하면서 (분류에 쓰인다~) 뭐시기. 근데 이거 문제가 overconfidence이기 때문에
정규화를 시킬거다 (정규화 방법 원리와 수식으로 정리) 후 logistic regression 돌린 결과 사진으로 보여주고 코드 첨부

- built 모델을 평가하기 위해 어떤 걸 쓰는지 Kaggle에서는 AUC를 쓴다~ 이건 어떤건지 ~~ 설명하고 실제 캐글에 제출한 모델 결과 첨부 + 모델 평가에 쓰이는 다른 방법들 (Komogornolv simir + gini value) 도 설명 뒤에 데이터에 넣어서 결과 보여주기

- 결론 지어주기.

- 전반적으로 추가된 내용이 있을텐데 그 부분을 반영해서 title과 subtitle, intro 수정해주기.


## EDA  
코드 설명하는 건 jupyter open note를 첨부하는 것으로 갈음하고, (따라서 )
여기부터는 코드의 result만 보여주고 전체적인 리서치 흐름으로 가기.
왜냐하면 이 블로그 포스팅 프로젝트의 핵심은 내가 crm 흐름을 이해하고 실제 데이터를 통해 의미있는 결과를 뽑아낼 수 있다는 것이지. 남들 다쓰는 코드를 친다는게 아님. 테크니컬한 측면이 아니라 실제 내용과 이런 목적에도 중대한 영향을 주는 경우의 코드의 경우에는 부록에 넣어두기. (WoE 계신 관련 수식 이후에 실제 코드로는 이렇게 작성된다를 보여준다거나~)

jupyter notebook도 이 블로그 포스팅의 흐름을 따라서 만들기