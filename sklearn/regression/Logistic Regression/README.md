
# 출처
https://ratsgo.github.io/machine%20learning/2017/04/02/logistic/

# 로지스틱 회귀란(8/26)?
지스틱 회귀는 선형 회귀 분석과는 다르게 종속 변수가 범주형 데이터를 대상으로 하며 입력 데이터가 주어졌을 때 해당 데이터의 결과가 특정 분류로 나뉘는 회귀.

# 증명에 필요한 개념

증명하기 앞서 우리가 필수적으로 알아야 하는 개념을 소개하겠다.

## 로지스틱 함수
우선 우리는 로지스틱 함수에 대해 알아야 한다.

로지스틱 함수는 분야에 따라 시그모이드 함수라고 불리기도 하며, 우리가 딥러닝에서 활성함수를 공부하며 나온 함수다.

![image](https://user-images.githubusercontent.com/95357946/186901169-b7e5de71-34ab-40d7-9f90-aaaa5bfc689e.png)

위 그림을 참고하면 로지스틱함수란 출력 결과가 항상 0과 1사이에 존재한다는 것을 알 수 있다.

## 승산

승산이란 임의의 사건 A가 발생하지 않을 확률 대비 일어날 확률의 비율을 의미히며,

![image](https://user-images.githubusercontent.com/95357946/186901658-e6d17fd9-4216-4917-a7d4-82f3a64fc1de.png)

이때 확률은 1에 가까울수록 승산이 높아지고, 사건의 발생확률이 높아지므로

![image](https://user-images.githubusercontent.com/95357946/186901910-922fcfbe-95ab-4e03-878d-a9524539a69b.png)

이런 그래프 모양을 가진다.

## 로짓 변환

승산에 log를 씌운 형태다. 

![image](https://user-images.githubusercontent.com/95357946/186902326-6b362dff-2d19-4603-9631-7871a33f0d22.png)

이후 이것은 직선의 형태를 띈다.

# 로지스틱 함수 증명
우리는 승산에 로그를 씌우면 직선의 형태가 되는 것을 위에서 알았다.

그럼 

![CodeCogsEqn](https://user-images.githubusercontent.com/95357946/186903874-a91ca3d8-785c-43d9-9af6-934a6d9a6322.svg)

이 수식이 성립한다.

이걸 풀어보면 

![image](https://user-images.githubusercontent.com/95357946/186904686-c7ab2172-0be7-44c8-abf6-8925a9e7de71.png)

![image](https://user-images.githubusercontent.com/95357946/186904858-743fbd9d-d370-4d96-9b39-a046329153da.png)

![image](https://user-images.githubusercontent.com/95357946/186905077-0892fd0d-c24b-41d1-bae0-1e24f0e7e8d5.png)

![image](https://user-images.githubusercontent.com/95357946/186905225-4c0acf5b-53d0-4d09-a7ef-0c13cd1a18f9.png)

![image](https://user-images.githubusercontent.com/95357946/186905587-a0776fc8-3b34-49e1-93b0-54c739fb3ad7.png)

![image](https://user-images.githubusercontent.com/95357946/186906471-48d89e88-0b76-4023-850f-c950f41d085b.png) 각자리에 나누면  

![image](https://user-images.githubusercontent.com/95357946/186906943-ce61b6d4-09dc-4778-86d3-df2b1a59daf6.png)

이걸 좀 정리하면 

![image](https://user-images.githubusercontent.com/95357946/186907056-ed66bc5b-28d0-4ec4-ac8e-8ae559be3215.png)


# 로지스틱 회귀 증명
여기서 어떤 변수 y=1에 대해서 있을 확률을 p라고 하자.

이때 범주 1에 속하는지 아닌지를 보려면 승산의 형태로 된 식이 필요하다

![image](https://user-images.githubusercontent.com/95357946/186907569-7481d972-0a9c-4542-8858-d1012756d83e.png)

그럼 이제 우리는 로짓을 이용하기 위해 양 변에 log를 씌어주고

![CodeCogsEqn](https://user-images.githubusercontent.com/95357946/186903874-a91ca3d8-785c-43d9-9af6-934a6d9a6322.svg)

이 수식이 성립한다.

이걸 풀어보면 

![image](https://user-images.githubusercontent.com/95357946/186904686-c7ab2172-0be7-44c8-abf6-8925a9e7de71.png)

![image](https://user-images.githubusercontent.com/95357946/186904858-743fbd9d-d370-4d96-9b39-a046329153da.png)

![image](https://user-images.githubusercontent.com/95357946/186905077-0892fd0d-c24b-41d1-bae0-1e24f0e7e8d5.png)

![image](https://user-images.githubusercontent.com/95357946/186905225-4c0acf5b-53d0-4d09-a7ef-0c13cd1a18f9.png)

![image](https://user-images.githubusercontent.com/95357946/186905587-a0776fc8-3b34-49e1-93b0-54c739fb3ad7.png)

![image](https://user-images.githubusercontent.com/95357946/186906471-48d89e88-0b76-4023-850f-c950f41d085b.png) 각자리에 나누면  

![image](https://user-images.githubusercontent.com/95357946/186906943-ce61b6d4-09dc-4778-86d3-df2b1a59daf6.png)

이걸 좀 정리하면 

![image](https://user-images.githubusercontent.com/95357946/186907056-ed66bc5b-28d0-4ec4-ac8e-8ae559be3215.png)

으로 로지스틱 함수와 유도 과정이 유사하다.



# 로지스틱 결정 계수
만약 y=1의 범주에 속한다면 범주에 속할 확률인 p는 p > 1-p가 성립한다.

![image](https://user-images.githubusercontent.com/95357946/186908589-2e788dd6-49ff-4098-a940-ff75bb0d41df.png)

이후 양변에 log를 씌우고 log1은 0이니까

![image](https://user-images.githubusercontent.com/95357946/186908772-440790b1-fb5d-4172-846b-e27613b00bf4.png)

역시 성립한다.

그리고 우리는 이미 로짓을 통해서 
![image](https://user-images.githubusercontent.com/95357946/186909112-74223928-c3ee-4ab6-a069-def5fd4278f8.png)

이것이 성립함을 알기때문에 우린 이것이 직선을 가진다는 것을 이미 알고 있다.

따라서 이걸 좌표 평면으로 이해 한다면

![image](https://user-images.githubusercontent.com/95357946/186909197-6dbc4bd3-3ef5-47f2-ba6b-d000d378ddb8.png)

이런 그림이 나온다.

이때 직선은 

![image](https://user-images.githubusercontent.com/95357946/186909319-1978f61e-09b5-4c28-a8b1-246598002f7d.png)

0이 되는 지점을 지나므로 하나의 경계가 생긴다.

이때 이 직선을 결정 경계라고 하고 이 식보다 크면 위에, 작으면 직선의 아래애 분포한다. 즉 만일 위에서 든 예시처럼 y=1이라는 범주에 들어 간다면 직선 위에 포함되고 아니라면 직선 아래에 포함된다.

# 다항 로지스틱 회귀
다항 로지스틱 회귀는 활성함수인 소프트맥스 함수와 매우 연관이 있다. 증명해보자.

이때 우리는 y=1, y=2, y=3의 범주가 3개라고 하자.

범주가 3개이므로 다음이 성립한다.
![Screenshot 2022-08-26 at 22 05 36](https://user-images.githubusercontent.com/95357946/186910113-bfa27b45-b1e6-4a8a-a3a2-8555870cd7ad.JPG)

정리하면
![Screenshot 2022-08-26 at 22 06 39](https://user-images.githubusercontent.com/95357946/186910372-b2e13baf-1791-4cbd-b958-e002b7c7f82f.JPG)

이걸 표준화 한다면 주어진 범주 k개가 주어질때

![Screenshot 2022-08-26 at 22 08 09](https://user-images.githubusercontent.com/95357946/186910595-a9074e87-1f3f-4fc7-abf9-6fd9e5b5b829.JPG)

이 식이 성립한다.
