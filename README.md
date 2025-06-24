# Optimal-Site-Selection-for-EV-Charging-Stations
전기차 충전소 최적 입지 선정

# 데이터 전처리

- Geometry 데이터를 WGS84 좌표계와 GRS80 직각좌표계로 변환

![image](https://github.com/user-attachments/assets/70049d9c-d6aa-4a5f-b258-4fedab7742e2)

- 인구수, 건축물수, 주거용면적, 건축물 높이, 건축물 연면적 데이터는 100m x 100m인 격자로 이루어져 있으며 모두 GRS80 좌표계를 사용하고 있으므로 통일성을 위해 WGS84 좌표계를 GRS80으로 변환

![image](https://github.com/user-attachments/assets/819f989b-0c6e-4121-b926-ae7a1803d5da)

- 주소만 표기된 데이터는 카카오맵 API를 활용하여 위도, 경도 데이터 생성

![image](https://github.com/user-attachments/assets/594bb8b3-ce5b-4cb9-b3bd-18c3ad17604c)

- 충전소가 세종시 격자 내에 존재 여부 확인 및 좌표계 데이터 생성

![image](https://github.com/user-attachments/assets/91be46ac-a002-40c9-b164-e6495bd3067d)
![image](https://github.com/user-attachments/assets/7c6b17c1-aab3-4772-8c60-4b11a6450bad)

- 세종시 데이터 변환 결과

![image](https://github.com/user-attachments/assets/a519e215-ed8d-458e-860a-519101fcf3e7)

- 데이터셋이 부족하므로 세종시 데이터셋은 Testset으로 활용, 이외 전기차 등록이 많은 세종시와 인접한 지역을 찾아 Trainset 전처리

![image](https://github.com/user-attachments/assets/17172a7b-ff00-474e-8edb-e9ab46b45aed)
![image](https://github.com/user-attachments/assets/1ead316e-4264-4074-bb1d-78430427450b)

- Trainset과 Testset 각각 label의 비율이 0.985 : 0.015, 0.993 : 0.007로 불균형

![image](https://github.com/user-attachments/assets/3ba1ddbd-e88e-4dca-84fd-266d7c001917)

- 언더샘플링과 오버샘플링을 통해 데이터증강

![image](https://github.com/user-attachments/assets/7e621684-514c-4851-bce5-924fb9ced20d)

<br/>

# 모델 적용 및 평가
- 우리는 충전소가 없는 위치에 충전소가 있다고 모델이 분류한 것을 최적입지로 선정하기 위해 Fals Positive를 활용
- 데이터 증강을 통해 label을 균형있게 맞췄으나 데이터 자체는 매우 불균형하기에 평가를 F1 Score와 Micro 평균으로 평가
- GrideSearchCV를 활용하여 최적의 파라미터를 구하려 하였으나 K-Fold Cross Validation에서 불균형한 데이터셋은 데이터가 고르게 분포되지 않을 수 있기에 OPTUNA를 적용하여 최적의 하이퍼파라미터를 구함
- 평가
  - Random Forest (F1 Score가 0.9를 넘는 모델들의 최적 입지는 총 22곳)

  ![image](https://github.com/user-attachments/assets/dabb76f7-0225-4212-a6a4-499d9fe8c43e)
  ![image](https://github.com/user-attachments/assets/567c7377-8b0f-4665-9e88-16dde11b7ad7)
  
  - LightGBM (F1 Score가 0.9를 넘는 모델이 없어 제외)

  ![image](https://github.com/user-attachments/assets/853be93d-8eec-47a0-bd55-2433a5793dc2)
   
  - Catboost (Catboost는 언더샘플링에서 매우 우수한 성능을 보임 최적 입지는 총 19곳)
  
  ![image](https://github.com/user-attachments/assets/be8af633-9b40-42d0-9611-f9251632076f)

  - Deeplearning (준수한 성능을 보였으며 Thresholde가 오를 수록 높은 성능을 보임, 최적 입지는 총 24곳)

  ![image](https://github.com/user-attachments/assets/755fce47-b1fd-4f24-ab75-ddbdeae09541)

<br/>

# 결과
- 세종시 내 최적 입지 우선 순위로 13곳이 선정됨

![image](https://github.com/user-attachments/assets/1322aafa-e29c-4226-b174-ef532b8e7e79)

- 기존 입지(빨강)과 최적 입지 추천 장소(파랑) 비교

![image](https://github.com/user-attachments/assets/1366fd0f-a63a-4aeb-a328-fccb1d164dc4)

<br/>
