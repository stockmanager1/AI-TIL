# 분석
1. 출처 : https://dacon.io/competitions/official/235959/codeshare/6041?page=1&dtype=recent

레퍼런스 1. 출처 : https://dacon.io/competitions/official/235959/codeshare/6507?page=1&dtype=recent

레퍼런스2. 출처 : https://dacon.io/competitions/official/235959/codeshare/6472?page=1&dtype=recent

# 1번에서 느낀점(8/24~8/26)
1. pycaret을 통해서 처음으로 auto ml을 접하게 되었다.

  자동으로 모델을 선정하는 것이 매우 인상적이었다. 
  
  pycaret에 대한 기본적인 흐름정도는 잡고 넘어가도 매우 좋은 라이브러리 같다.

2. info()문을 통해서 결측치를 미리 알 수 있다는 것이 매우 흥미로웠다.
  
  원래 결측치에 대해 별로 생각도 없었고, 그저 그런 개념이 있구나 하고 넘어 갔었는데, 이번 기회에 왜 결측치를 처리해야 하는지 확실히     알았고 정말 여러가지 방법으로 처리하는 것을 알았다.그리고 결측치의 갯수에 따라 처리 방법이 달라지는 것 역시 꼭 참고하면 좋을것 같     다.
 
3. EDA(수집한 데이터가 들어왔을 때, 이를 다양한 각도에서 관찰하고 이해하는 과정)라는 과정에 대해 알게되었다.
  
  단순히 전처리라고만 생각했지 복합적으로 결과를 도출해야 하는 사고 방식 과정을 알게되어 앞으로 정말 유용하게 사용할 것 같다.
  
 4. 전반적인 분석 과정에서 틀을 잡게 되었다.
 
          1. 라이브러리 불러오기

          2. 데이터 불러오기

          3. EDA  & 전처리

            3.1 EDA

                 3.1.1 수치형 데이터

                 3.1.2 범주형 데이터
  
           3.2 전처리

                 3.2.1 결측치 처리

                 3.2.2 스케일링

                  3.2.3 인코딩

         4. 모델링

                4.1 split

                4.2 성능 검증용 함수

                4.3 모델 automl로 선정

        5. 제출

 이런 틀로 분석이 시행됨.
 
  5. 데이터 인코딩과 스케일에 대해 알아야 겠다.
    
    카테고리형 속성은 데이터를 labelencoder를 해야 한다.
    
    각 스케일과 인코딩에 대해 한번 더 공부하자
    
  6. 여기서 평가함수를 만들때 매우 흥미로운 레퍼런스를 사용한다. 

    꼭 다시 분석하자.

# 레퍼런스1에서 느낀점(10/22)

eda의 과정에 대해 분석했다.

각 변수에 대해 정확히 분석하는 방법을 정립했다.

만일 변수가 포괄적으로 나온다면 bin을 4개에서 5개로 묶어서 새로운 변수를 만들어 해결했다.

파생변수에 대해 좀 더 알게 되었다.


# 레퍼런스2에서 느낀점(10/30)
약간 진짜 통계적으로 접근한 관점이다. 

## missingno 모듈을 사용해서 결측치를 한번에 시각화 했다.

## 데이터의 결측치, 고유값등을 한번에 불러오는 함수

```
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
```
## 타겟함수를 더욱더 심층적으로 분석했다.

```
def write_percent(ax, total_size):
  # 도형 객체를 순회하며 막대 그래프 상단에 타깃값 비율 표시
  for patch in ax.patches:
    height = patch.get_height() # 도형 높이(데이터 개수)
    width = patch.get_width() # 도형 너비
    left_coord = patch.get_x()  # 도형 왼쪽 테두리의 x축 위치
    percent = height/total_size*100 # 타깃값 비율

    # (x,y) 좌표에 텍스트 입력
    ax.text(left_coord + width/2.0,
            height + total_size*0.001,
            '{:1.1f}%'.format(percent),
            ha='center')
    
mpl.rc('font', size=15)
plt.figure(figsize=(7,6))

ax = sns.countplot(x='ProdTaken', data=train)
write_percent(ax, len(train))
ax.set_title('Target Distribution')
```
prodtaken이 타겟겂임. 이것만 바꿔써라.

## 타겟 1의 값이 각 고유값에 차지하는 비율을 구하는 함수

```
# 각 고유값마다 차지하는 Target값의 1이 차지하는 비율 그래프
def plot_target_ratio_by_features(df, features, nrows, ncols, size=(6,12)):

  mpl.rc('font', size=9)
  plt.figure(figsize=size)
#gridspec: 여러 axes를 한번에 배치가능
  grid = gridspec.GridSpec(nrows=nrows, ncols=ncols)
  plt.subplots_adjust(wspace=0.3, hspace=0.3)

  for idx, feature in enumerate(features):
    ax = plt.subplot(grid[idx])
    sns.barplot(x=feature, y='ProdTaken', data=df, palette='Set2', ax=ax)
```
그래프에서 둘 사이의 비율이 비슷하거나 선의 길이(신뢰구간)의 길이가 긴 경우에 그 변수들을 탈락. 

높은 신뢰도----> 신뢰구간이 짧다.

낮은 신뢰도----> 신뢰구간이 길다.

## 교차분석을 사용해서 데이터를 분석한다.
[crosstab 설명](https://suy379.tistory.com/149)

```
## 교차분석표

def get_crosstab(df, feature):
#normalize = index를 사용했으니까 이건 target으로 설정한 protaken의 비율을 알 수 있다.
#*100을 하는 이유는 퍼센테이지로 보기위해
  crosstab = pd.crosstab(df[feature], df['ProdTaken'], normalize='index') *100
  crosstab = crosstab.reset_index()
  return crosstab
```
order를 사용하면 x축의 순서를 임의로 변경한다. twinx를 사용하면 축이 헷갈릴 수 있어서 order로 지정해주면 좋다.
```
def plot_pointplot(ax, feature, crosstab):
# 두 개의 y 축이있는 플롯이 twinx()

  ax2 = ax.twinx()
#pointplot은 막대를 통해 편차와 신뢰구간를 표현합니다.
  ax2 = sns.pointplot(x=feature, y=1, data=crosstab, order=crosstab[feature].values,
                      color='black', legend=False)
  #우리는 1의 비율을 알고 싶기 때문이다. crosstab[1]이 1의 비율을 나타내는 부분을 의미한다.
  ax2.set_ylim(crosstab[1].min()-5, crosstab[1].max()*1.1)
  ax2.set_ylabel('Target 1 Ratio(%)')
```
[seaborn 그래프 정리](https://coding-grandpa.tistory.com/62?category=954731)
```
#최종적인 교차분석 함수
def plot_cat_dist_with_true_ratio(df, features, num_rows, num_cols, size=(15,20)):

  plt.figure(figsize=size)
  ##gridspec: 여러 axes를 한번에 배치가능
  grid = gridspec.GridSpec(num_rows, num_cols)
  plt.subplots_adjust(wspace=0.45, hspace=0.3)

  for ids, feature in enumerate(features):
    ax = plt.subplot(grid[idx])
    crosstab = get_crosstab(df, feature)

    sns.countplot(x=feature, data=df, order=crosstab[feature].values, color='skyblue', ax=ax)

    write_percent(ax, len(df))

    plot_pointplot(ax, feature, crosstab)

    ax.set_title(f'{feature} Distribution')
```

## 파생변수를 만든다.
```
# TypeofContact의 값에 따라 추출
#파생변수를 만들어버림. 여기서는 loc를 사용해서 typeocontact에서 conmpany invited, self로 나눔.
#이러면 TRAIN에 미포함 시킨채로 새로운 뱐수를 만들 수 있다.
train_c = train.loc[train['TypeofContact'] == 'Company Invited']
train_s = train.loc[train['TypeofContact'] == 'Self Enquiry']
```

## 전처리 과정은 결국 머신러닝에 사용할 피처들을 선택하고 파생변수들을 새로 만드는 과정
