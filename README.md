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
    
- Data : 초기 데이터와 모델링 데이터 셋이 들어있는 폴더입니다.(초기 데이터 셋은 
    - air_2021.csv, air_2022.csv : 대기 질에 관한 정보가 들어있는 초기 데이터 셋
    - weather_2021.csv, weather_2022.csv : 날씨에 대한 정보가 들어있는 초기 데이터 셋
    - train.csv : 모델 학습을 위해 정제한 데이터 셋
    - test.csv : 예측을 위해 정제한 데이터 셋
    
- Model : 모델링에 대한 정보가 담겨있는 폴더입니다.
    - DataBase_For_Modeling : 시계열적 특성을 뽑아낸 데이터 셋
    - Model_Save : 학습시킨 모델을 저장한 파일
      - Baseline.7z : 초기 모델(variance importance를 고려하지 않고 모든 변수를 넣어서 학습시킨 모델)
      - Remove_importance_variables.7z : variance importance를 고려하여 중요한 변수만 넣어서 학습시킨 모델
    - Baseline.ipynb : 초기 모델(variance importance를 고려하지 않고 모든 변수를 넣어서 학습시킨 모델) 코딩 파일
    - Remove_not_important_variable_and_Ensemble.ipynb : variance importance를 고려하여 중요한 변수만 넣어서 학습시킨 모델의 코딩 파일
 
- Preprocessing : 데이터 전처리에 대한 정보가 담겨있는 폴더입니다.
    - autocorrelation.ipynb : 예측값(PM10)이 과거 몇 시점까지 상관성이 있는지 파악하기 위한 코드
    - preprocessing.ipynb : 데이터 전처리 함수들이 담겨있는 코드
    - TimeSeriesDecomposition.ipynb : 시계열 분해를 진행하고, 시계열 분해요소를 추출하는 

## 문의사항
* email : ajc227ung@gmail.com
