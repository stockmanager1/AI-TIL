# 머신러닝 베이스 라인 세우기

출처 : 여러 레퍼런스를 분석하거나, 여러 서적을 참고해서 만든 베이스라인

## 라이브러리 불러오기

```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

import warnings
warnings.filterwarnings("ignore")

```
## 데이터 불러오기

```
!unzip -qq '/content/dataset.zip' # 압축파일 경로

train = pd.read_csv('/content/data/train.csv')

test = pd.read_csv('/content/data/test.csv')

```


## eda전 데이터 살피기

```
# 결측치를 살피는 함수
import missingno as msno

train_copy = train.copy()
msno.bar(df=train_copy.iloc[:, 1:20], figsize=(13,6))

```

```
# 데이터 모양을 보여주는 함수
def resumetable(df):
  print(f'데이터셋 형상: {df.shape}')
  summary = pd.DataFrame(df.dtypes, columns=['데이터 타입'])
  summary = summary.reset_index()
  summary = summary.rename(columns={'index':'피처'})
  summary['결측값 개수'] = df.isnull().sum().values
  summary['고윳값 개수'] = df.nunique().values
  summary['첫번째 값'] = df.loc[0].values
  summary['두번째 값'] = df.loc[1].values
  summary['세번째 값'] = df.loc[2].values

  return summary
  
#칼럼별 고유값 불러오기
for col in train.columns:
  cols = str(col)
  print(f'{col} 고유값 : {train[cols].unique()} \n')
```
## 대략적인 eda

```
# 타겟에 대한 각 변수의 분포

import matplotlib.gridspec as gridspec

# 3행 2열 틀 준비
mpl.rc('font', size=12)
grid = gridspec.GridSpec(3,2)  # 그래프(서브플롯)를 3행 2열로 배치
plt.figure(figsize=(10,16)) # 전체 Figure 크기 설정
plt.subplots_adjust(wspace=0.4, hspace=0.3) # 서브플롯 간 좌우상하 여백설정

# 서브플롯 그리기
bin_features = ['TypeofContact', 'Gender', 'MaritalStatus', 'Passport', 'OwnCar']

for idx, feature in enumerate(bin_features):
  ax = plt.subplot(grid[idx])

  # ax축에 타깃값 분포 카운트 플롯 그리기
  sns.countplot(
      x=feature,
      data=train,
      hue='ProdTaken',
      palette='pastel',
      ax=ax)

  ax.set_title(f'{feature} Distribution by Target' )
  write_percent(ax, len(train))
```
![image](https://user-images.githubusercontent.com/95357946/209304654-73a9d5b2-cec5-4451-bff0-56086ef268ea.png)
