# 래퍼런스 3 eda 과정 분석

[출처](https://dacon.io/competitions/official/236010/codeshare/6961?page=1&dtype=recent)

## 분석적 특이점
데이터로 EDA를 진행하고 상관관계에 따른 분석이 아니라 따로 세울 수 있는 경우의 수를 스스로 가설로 만들어서 하나씩 검증해나감. 이런 과정 속에서 필요없는 데이터와 병합해야 하는 데이터를 찾아냄.

## 기술적 특이점

### 1. 서로 다른 변수들을 비교, 분석할때, 막대 그래프로 정말 정밀하고, 시각적으로 분석함.

![image](https://user-images.githubusercontent.com/95357946/199265515-2820791d-82ae-4a1f-8e90-6dd017ddb3bb.png)

```
# 태양광 발전량이 30미만인 날과 30이상인 날의 상대습도 평균 비교
x = ['발전량<30인 날', '발전량>=30인 날']
y = [train[train['TARGET'] < 30]['RH'].mean(), train[train['TARGET'] >= 30]['RH'].mean()]

plt.figure(dpi=150)
plt.title("태양광 발전량 vs 상대습도")
plt.xlabel('발전량')
plt.ylabel('상대습도')
plt.bar(x, y)
plt.show()
```

이런식으로 막대그래프로 비교분석하는 방법도 존재했다.

### 2. 산점도 그래프로 데이터의 모습과 방향을 알 수 있다.

![image](https://user-images.githubusercontent.com/95357946/199266001-79882a8e-b3f4-483d-b8e4-b2b038f26efc.png)

```
# 태양광 발전량이 10미만인 날
x_0 = train.loc[train['TARGET']<10, 'TARGET']
y_0 = train.loc[train['TARGET']<10, 'RH']

# 태양광 발전량이 10이상 30미만인 날
x_1 = train.loc[(train['TARGET']>=10)&(train['TARGET']<30), 'TARGET']
y_1 = train.loc[(train['TARGET']>=10)&(train['TARGET']<30), 'RH']

# 태양광 발전량이 30이상인 날
x_2 = train.loc[train['TARGET']>=30, 'TARGET']
y_2 = train.loc[train['TARGET']>=30, 'RH']


# scatter 산점도 그래프 그리기
plt.figure(dpi=150) 

plt.title('발전량 vs 상대습도')
plt.xlabel('발전량')
plt.ylabel('상대습도')

plt.scatter(x_0, y_0)
plt.scatter(x_1, y_1)
plt.scatter(x_2, y_2)

plt.show()
```

### 서브 플롯으로 빠르게 그래프 모두 시각화하기
![image](https://user-images.githubusercontent.com/95357946/199266364-ed65ab93-133b-4af9-821c-fe68f01c47fb.png)

```
# 시각화를 위한 라이브러리
import seaborn as sns

fig, ax = plt.subplots(2, 4, figsize=(16, 5))
for idx, feature in enumerate(train.columns):
    if idx<4//2+1:
      sns.distplot(train[feature], ax=ax[0,idx], bins=50)
    else:
      sns.distplot(train[feature], ax=ax[1,idx-4], bins=50)
```

enurmate을 사용해서 각 그래프의 숫자를 할당. 이후 이 숫자들을 가지고 정렬함. 이 if와 else문으로 정렬한걸 꼭 유념히 살펴보
