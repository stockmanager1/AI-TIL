
# 정보이론

정보량이란 단일 사건에 대한 정보량으로 깜짝놀라는 정도를 의미

로또를 맞을 확률 --> 거의 불가능한 사건

일어난다면 깜짝 놀라는 정도가 매우 높다.

로또에 떨어질 확률 -----> 매우 높다---> 즉 새롭게 얻을 정보가 없다.

즉 정리하자면 

정보량이 높다면---> 확률이 떨어지고

정보량이 낮다면 ---> 확률이 높아진다.

이걸 그래프로 구현 한다면 놀람의 정도I(x)가 낮을수록 확률이 낮아지고, 놀람의 정도 I(x)가 높을수록 확률은 높아진다.

즉 로그함수가 나온다.

![image](https://user-images.githubusercontent.com/95357946/188795340-03e43fd1-eded-46b4-b410-d15a37e65a6f.png)

이걸 공식으로 구현한다면 정보량 I(x)는

![image](https://user-images.githubusercontent.com/95357946/188795144-6876234b-0de2-421a-ad73-5d8f4a26a6b9.png)

# id3 알고리즘
즉 id3알고리즘이란 트리구조 알고리즘으로 entropy 값의 변화량(information gain)을 나타내어 트리로 분류하는 알고리즘을 의미한다.

cart알고리즘과 다르게 3개 이상의 branch를 생성한다. 

skelarn은 cart만 지원

# id3 알고리즘의 구현 순서

1. 아무런 분기가 알이나지 않은 상태의 엔트로피
2. 각 지표별 분기 진행
3. 각 경우의 information gain을 측정하고 가장 큰 information gain을 기준으로 분기
4. 반복
5. 의사결정 트리 완성

# 엔트로피
어떤 사건에 대한 확률 분포, 즉 정보량에 대한 평균

![image](https://user-images.githubusercontent.com/95357946/188796918-04a8f13a-529e-4481-9bf6-b709b797f7d9.png)

공식을 해석하자면 사건의 정보량 * 해당 정보의 확률

# information gain
entropy 값의 변화량을 나타내기 위해 사용

즉 id3은 information gain이 높은 것을 선택해서 트리구조를 만들어 간다.

분기전 엔트로피 H(S)

분기후 true 그룹의 엔트로피 H(s,true)

분기후 false 그룹의 엔트로피 h(s,false)

이때 각각의 information gain은

h(x) - H(s,true)
h(x) - h(s,false)

으로 구해야 한다.

# 예시로 id3를 알아보자

![Screenshot 2022-09-07 at 14 44 14](https://user-images.githubusercontent.com/95357946/188797984-e1145e54-8ce9-4190-ac4d-03986aba6f15.JPG)

play에 대한 분기를 알아보자.

이때 play가 yes, no 두가지로 나누어지기 때문에 

![image](https://user-images.githubusercontent.com/95357946/188798579-cc053678-ca80-4a25-98c1-03f486981e5e.png)

![image](https://user-images.githubusercontent.com/95357946/188798700-3f1f4a41-2cae-4051-a47b-c9fe6a6f5c24.png)

= 0.94

즉 아무것도 일어나지 않은 상태의 엔트로피는 0.94를 의미한다.

outlook
![image](https://user-images.githubusercontent.com/95357946/188799087-8f1f87df-94d0-4dc2-84d6-289e10cea91f.png)
![Screenshot 2022-09-07 at 14 55 10](https://user-images.githubusercontent.com/95357946/188799391-c1b4ad15-ebbc-4e4d-bbe2-bb472f9bc6ba.JPG)

이제 이 과정을 temperature, humidity, windy역시 이런 과정을 거친다.

이후 이걸 분기전 엔트로피에서 각각을 뺴준다면 

이때 가장 큰 값이 0.25로 outlook이 가장 큰것을 알 수 있다.

따라서 outlook에 대해서 분기를 해야 한다.

outlook는 다시 3가지로 나누어 지기 때문에

![image](https://user-images.githubusercontent.com/95357946/188800017-c15adfc1-e014-4805-a4d3-1b8448fa210b.png)

먼저 sunny에 대해 분기한다면 다음분기는 humidity로 진행된다

이걸 반복하면 

![image](https://user-images.githubusercontent.com/95357946/188800188-261ab9ef-c7bd-4582-b997-13cd90b3c475.png)

# 출처
https://tyami.github.io/machine%20learning/decision-tree-2-ID3/




