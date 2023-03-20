# <b>Amazon business analytics hackathon</b>

## 개요
  코로나 19 확산 이전부터 1인 가구 증가로 인한 배달 산업이 증가하고 있는 추세이다. 치킨집만 배달을 하던 시대에서 코로나 19 확산으로 인해 배달을 하지 않는 가게를 찾는 것이 더 어려운 시대로 변화하고 있다. Figure 01과 같이 외식업체들의 배달 플랫폼 이용률이 4년만에 5배가 올랐다.<br>
  
  ![배달앱 사용 비중](http://www.sisajournal-e.com/news/photo/202204/264716_108396_5435.jpg )<br>
 [Figure 01] 배달앱 사용 비중 변화<br>
 
  우리가 흔히 사용하고 있는 ‘배달의 민족’과 같은 배달 플랫폼에서 소비자에게 배달 예상 소요 시간을 제공하고 있다. 이 예상 시간은 음식 주문을 받은 점주의 '감'에 의존한 정보였다. 그날의 주문량이나 상황에 따른 점주의 주관적인 판단에 기반했기 때문에 정확도가 떨어졌다. 그래서 최근 배달 플랫폼에서는 배달 수행 빅데이터를 기반으로 주문자와의 거리까지 반영해 보다 정확한 배달 예상 소요 시간을 산출하고자 하고 있다. <br>

  하지만 더 좋은 정확도를 위해서는 주문자와의 거리뿐만 아니라 배달에 영향을 미치는 요인들을 합쳐 더 합리적인 방법으로 배달 소요 시간을 예측하여야 한다.<br>
  
  따라서 ‘아마존’ 배달 데이터를 활용하여 배달 소요 시간에 영향을 미치는 요인을 분석하고 배달 소요 시간을 예측하는 모델을 제작함으로써 점주에게는 더 효율적인 배달 환경을 구축하기 위한 피드백을, 소비자에게는 더 정확한 배달 예상 소요시간을 제공하고자 한다. 
  
  ## DATASET

<b>Delivery time Test.cvs</b>
- Source.name.1
- ID
- Delivery_person_ID
- Delivery_person_Age
- Delivery_person_Ratings
- Restaurant_latitude
- Restaurant_longtitude
- Delivery_location_latitude
- Delivery_location_longtitude
- Time_Orderd
- Time_Order_picked
- Weather conditions
- Road_traffic_density
- Vehicle_condition
- Type_of_Order
- Type_of_vehicle
- multiple_deliveries
- Festival
- City

<b>Delivery time.cvs</b>
- Source.name.1
- ID
- Delivery_person_ID
- Delivery_person_Age
- Delivery_person_Ratings
- Restaurant_latitude
- Restaurant_longtitude
- Delivery_location_latitude
- Delivery_location_longtitude
- Order_Date
- Time_Orderd
- Time_Order_picked
- Weather conditions
- Road_traffic_density
- Vehicle_condition
- Type_of_order
- Type_of_Vehicle
- Multiple_deliveries
- Feastival
- City
- Time_taken(min)

## 한계
  본 프로젝트에서 사용한 데이터의 위동와 경도로 추측해본 결과 인도 데이터인 것을 확인하였다. 위도와 경도로 거리를 찾는 과정에서 Google map API를 사용했는데 인도의 분쟁지역에 해당하는 장소가 30%정도 포함되어 제거하였다.  
또한 데이터 예측에 필요하지 않은 컬럼, 예를들어 ID, vehicle, ranking을 다수 포함하고 있어 예측에 필요한 컬럼이 적다는 점에서 모델의 정확성이 높게 측정되지 않았던 것 같다. 휴일인 경우, 배달 소요 시간에 영향을 끼칠 거라고 생각하여 휴일 api를 사용하였지만 2022년 2월 ~ 4월 데이터라서  holiday의 분포가 너무 적었다. Multiple_deliveries와 festival의 경우도 큰 영향을 끼치는 feature라고 생각하였지만 분포의 불균형이 심했다.


