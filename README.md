# 미세먼지 농도 예측

## 개요
* 본 프로젝트는 KT 에이블스쿨 2차 미니프로젝트에서 진행하였습니다.

## 배경
* 본 프로젝트는 서울시 종로구의 1시간 후 미세먼지 농도(PM10)를 예측하는 것이 목표였습니다.
* 구체적인 미세먼지 측정 장소는 서울 종로구 종로35가길 19입니다.
* 해당 주소에는 미세먼지 측정소가 있으며, 이 한 곳의 미세먼지 농도를 예측해야 했습니다.

## 개발환경
<p align="center">
  <img src="https://img.shields.io/badge/pandas-150458?style=flat-square&logo=pandas&logoColor=white"/></a>&nbsp
  <img src="https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=NumPy&logoColor=white"/></a>&nbsp
  <img src="https://img.shields.io/badge/scikit-learn-F7931E?style=flat-square&logo=scikit-learn&logoColor=white"/></a>&nbsp
  <img src="https://img.shields.io/badge/JSON-000000?style=flat-square&logo=JSON&logoColor=white"/></a>&nbsp
  <br>
    <img src="https://img.shields.io/badge/-statsmodels-blue"/></a>&nbsp
    <img src="https://img.shields.io/badge/%20-request-black"/></a>&nbsp
    <img src="https://img.shields.io/badge/-catboost-yellow"/></a>&nbsp
    <img src="https://img.shields.io/badge/-matplotlib-blue"/></a>&nbsp
</p>

## 프로젝트 결과
- rmse : 3.32031, r2-score : 0.98
![image](https://user-images.githubusercontent.com/89781598/192284107-f20760dc-cde2-495a-aa68-2e78303736bd.png)

## 프로젝트 설명
- 해당 프로젝트는 시계열적인 방법을 최대한 적용하려고 노력하였습니다.
- 시계열적인 방법을 적용하기 위해서, 아래의 2가지 조치를 취하였습니다.
  - 범주형 자료를 제외한 모든 변수들에 대해서 Residual, Seasonal, Trend를 추출한후, 이를 새로운 변수로 지정
  - target의 자기상관성을 분석한 결과, 3시점 이전의 값만 현재의 값에 영향을 미친다는 사실을 발견한 후, 3시점 이후의 값을 새로운 변수로 지정

## 파일 구조
```
📦KT.PM10
 ┣ 📂Crawling
 ┃ ┣ 📂CrawlingData
 ┃ ┃ ┣ 📜sunrise_sunset_2021.csv
 ┃ ┃ ┗ 📜sunrise_sunset_test.csv
 ┃ ┣ 📂DataList
 ┃ ┃ ┣ 📜2021_datelist.csv
 ┃ ┃ ┗ 📜2022_datelist.csv
 ┃ ┗ 📜Code.ipynb
 ┣ 📂Data
 ┃ ┣ 📜air_2021.csv
 ┃ ┣ 📜air_2022.csv
 ┃ ┣ 📜test.csv
 ┃ ┣ 📜train.csv
 ┃ ┣ 📜weather_2021.csv
 ┃ ┗ 📜weather_2022.csv
 ┣ 📂Model
 ┃ ┣ 📂DataBase_For_Modeling
 ┃ ┃ ┣ 📜Baseline_results.zip
 ┃ ┃ ┣ 📜Feature_Importance.zip
 ┃ ┃ ┣ 📜Remove_not_important_features_results.zip
 ┃ ┃ ┣ 📜resid_test.zip
 ┃ ┃ ┣ 📜resid_train.zip
 ┃ ┃ ┣ 📜seasonal_test.zip
 ┃ ┃ ┣ 📜seasonal_train.zip
 ┃ ┃ ┣ 📜trend_test.zip
 ┃ ┃ ┗ 📜trend_train.zip
 ┃ ┣ 📂Model_Save
 ┃ ┃ ┣ 📜Baseline.7z
 ┃ ┃ ┗ 📜Remove_importance_variables.7z
 ┃ ┣ 📜Baseline.ipynb
 ┃ ┗ 📜Remove_not_important_variable_and_Ensemble.ipynb
 ┗ 📂Preprocessing
 ┃ ┣ 📜autocorrelation.ipynb
 ┃ ┣ 📜preprocessing.ipynb
 ┃ ┗ 📜TimeSeriesDecomposition.ipynb
```
## 파일 
- Crawling : 일출, 일몰 시간을 크롤링한 과정에 대해서 다룹니다.
    - CrawlingData : 일출, 일몰시간을 크롤링한 데이터가 담겨져 있습니다.
    - DataList : 크롤링할 시간에 대한 정보를 담고 있는 데이터입니다.
    - Code.ipynb : 크롤링한 코드를 다룹니다.
    
- Data : 초기 데이터와 모델링 데이터 셋이 들어있는 폴더입니다.
    
- Trial : 문제를 해결하기 위해 여러 모델링을 시도한 코드입니다.
    - AutoEncoder : AutoEncoder를 이용하여 모델을 구성해보았습니다.
    - LSTM : LSTM layer를 이용하여 장기기억을 끌고가면서 모델을 학습시키면 성능이 더욱 좋을 것이라고 판단하여 이를 이용해 모델을 구성해보았습니다.
    - SDCFEModel
        - Seperate Dense Concat Fold Ensenble Model
        - 연관이 있는 열을 하나로 묶고, 연관성이 있는 그룹들 각각을 Dense layer로 차원을 축소하여 잠재벡터를 만듦.
        - 이 후, 해당 잠재벡터들을 모두 병합한 후, KFOLD 5로 5개의 모델을 각각 생성한 후, 모델들을 저장하고 Ensemble Method를 사용하여 5개의 모델을 모두 앙상블학습 시킴.
    - UNET-Ensemble
        - UNET의 구조를 착안하여 만든 모델로, UNET의 구조처럼 잠재벡터를 단계적으로 만들어나가면서, 단계별로 잠재벡터를 형성한다.
        - UNET의 최하단에 진입했을 때, 다시 상단으로 올라가면서 예측값을 생성한다.
        - 이 때, 상단으로 올라오는 단계별로 하단에서 만든 잠재벡터들을 아래에서 위로 끌어오면서, 모델이 하단으로 가면서 잃어버린 정보량을 최대한 보전하려고 노력한다.
        - 이 후, 최상단에서는 최하단으로 진입하는 단계에서 단계적으로 생성한 잠재벡터들을 모두 고려한 결과값을 도출한다.

## 문의사항
* email : ajc227ung@gmail.com
