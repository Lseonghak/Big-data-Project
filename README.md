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

  ## 데이터 전처리
  
Test data에는 Order_data와 Time_taken(min)가 없어서 Train data만 사용했다.
데이터 전처리 전 Train data : 45593 rows x 21 columns

<b>1. 필요없는 컬럼 삭제</b>
Source.Name.1, ID, Delivery_person_ID, Delivery_person_Age, Delivery_person_Ratings : 
데이터에서 배달시간과 관계없는 배달 앱 사용자의 정보 data이기 때문에 삭제 

Time_Order_picked, Type_of_order : 음식의 조리 시간을 제외한 순수 배달 시간에는 관련이 없는 주문시간과 배달 시작시간, type of order 열은 삭제시켰다.

<b>2.	결측치 처리</b>

 
Weather conditions, Road_traffic_density, Time_Orderd:
결측치를 시각화 했을 때 같은 row의 데이터에서 결측지가 많다고 판단하여 삭제
multiple_deliveries : 
41522데이터 중 결측치가 897개인데 데이터의 수가 많고 배달시간예측에 중요하게 생각되는 컬럼이라 결측지는 삭제함.
festival :
festival은 배달 중 이동시간과 관계가 있는 데이터인데 배달시간과 상관관계가 높아 결측치는 제거하였다.
City:
장소에 따른 배달 시간의 상관관계가 높다고 판단하여 결측치를 제거하였다.

<b>3. 이상치 처리</b>

latitude와 longitude : 
현 데이터의 latitude와 longitude로 보아 인도의 데이터를 추정할 수 있다. 하지만 latitude와 longitude가 0,0인 장소는 인도가 아님으로 제거한다.

multiple_deliveries : 
배달 시 여러개를 한번에 배달하는 컬럼인데 0인 값은 결측치라 판단하여 삭제함
  
distance :
Google Map API로 거리를 얻었는데 바꾸는 과정에서 이상치(distance == 0)가 생긴다. distance는 배달시간예측에 가장 중요한 컬럼이기 때문에 결측치는 제거한다.




<b>4. feature engineering</b>

Longitude, latitude : 음식점의 위치와 배달 주문 위치의 절대적인 위치를 나타낸다. 하지만 배달을 위해서는 이 거리를 실제 도로상의 거리를 필요로 하기 때문에 Google Map API를 통해 도로상의 거리 distance로 컬럼을 추가했다. 그리고 longitude와 latitude는 필요 없으므로 삭제한다.
 
식사 : 
배달 주문 시간의 분포를 보았을때, 점심, 저녁시간대에 배달 주문이 몰려있는 것을 확인하였다. 
 
배달 주문 시간별 배달 소요 시간의 분포를 보니 배달 주문 분포와 비슷한 양상으로, 점심, 저녁 시간대에 더 오래걸린다는 것을 확인할 수 있었다. 

따라서 주문 시간을 통해 식사시간(11~14시, 17~21시)과 식사시간이 아닌 것으로 나누었다.

 
위에서 전처리를 거친 후 각 feature의 분포도를 출력해보면 다음과 같다. 이를 통해 알 수 있는 사실은 다음과 같다.
1.	배달 소요시간은 평균적으로 30분 정도이다.
2.	Multiple_deliveries, Festival, holiday 칼럼의 데이터 분포가 고르지 않다.
3.	식사 feature는 값이 1이면 식사시간, 0이면 식사시간이 아닌 시간대를 의미하는데, 이를 분석해보면 식사시간대에 배달주문량이 많음을 알 수 있다.

다음은 feature들의 상관계수를 heatmap으로 시각화한 결과이다. 
 
우리가 예측하고자 하는 Time_taken(min)과 상관계수가 높은 feature들의 상관계수를 살펴보면 다음과 같다.
	distance	multiple_deliveries	식사	Road_traffic_density_Jam	Road_traffic_density_Low	Festival
Time_taken (min)	0.557688	0.368384	0.691121	0.547791	-0.667516	0.24932

따라서 거리, 동시에 몇개의 배달을 하는지, 식사시간대인지, 교통상황, festival의 유무 등이 time_taken(min)에 영향을 많이 끼친다고 분석할 수 있다.
![image](https://user-images.githubusercontent.com/86548137/226331417-b51afc63-ec86-4c2e-85d6-d38b292e993a.png)

