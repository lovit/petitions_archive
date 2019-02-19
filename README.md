# 청와대 국민청원 데이터 아카이브

청와대 국민청원 [게시판](https://www1.president.go.kr/petitions)의 데이터를 월별로 수집합니다.

청원은 게시판에 글을 올린 뒤, 한달 간 청원이 진행됩니다. 수집되는 데이터는 `청원종료`가 된 이후의 데이터이며, 청원 내 댓글은 수집되지 않습니다. 단 청원의 동의 개수는 수집됩니다.

청원 데이터에 대해서는 [petitions_dataset repository](https://github.com/lovit/petitions_dataset) 를 참고하세요. 이 repository 는 데이터만을 보관합니다.

# 청와대 국민청원 데이터 활용법

아래의 사용 코드는 [https://github.com/lovit/petitions_dataset](https://github.com/lovit/petitions_dataset) 의 패키지의 사용법입니다.

## Fetch

설치된 패키지는 데이터를 가지고 있지 않습니다. `fetch` 를 이용하여 데이터를 다운로드 받습니다. 다운로드 기본 위치는 패키지가 설치된 위치입니다. 

```python
from petitions_dataset import fetch

fetch()
```

`data_dir` 에 다운로드 폴더의 위치를 지정할 수 있습니다.

```python
data_dir='./downloaded_petitions'
fetch(data_dir)
```

## Usage

다운로드 받은 데이터 폴더 위치를 `data_dir` 에 입력할 수 있습니다.

```python
from petitions_dataset import Petitions

petitions = Petitions()
petitions = Petitions(data_dir)
```

청원의 개수를 확인할 수 있습니다.

```python
len(petitions) # 277296
```

분석에 이용할 청원 기간을 설정할 수 있습니다. 형식은 `yyyy-mm-dd` 입니다.

```python
petitions = Petitions(data_dir, begin_date='2018-08-03', end_date='2018-11-28')
len(petitions) # 96956
```

Iteration 시 yield 되는 항목을 설정할 수 있습니다. 설정 가능한 항목은 아래와 같습니다.

```
['category', 'begin', 'end', 'content', 'num_agree', 'petition_idx', 'status', 'title', 'replies']
```

항목을 설정하면 설정된 값들이 tuple 의 형태로 출력됩니다.

```python
petitions.set_keys('date', 'category', 'title')

for date, category, title in petitions:
    print(date, category, title)
```

```
('2018-08-03', '안전/환경', '트럭 장사꾼 확성기 사용을 강력히 단속하고 어기면 벌금 많이 내게 해주세요.')
('2018-08-03', '정치개혁', '사법부 국정논단 국민은 애가 끊어집니다.')
('2018-08-03', '정치개혁', '한국인의 대통령이 되어주세요')
('2018-08-03', '기타', '전기료 한시적 누진제 완화는 저소득층에 대한 역차별 아닌가요?')
('2018-08-03', '보건복지', '국민연금 골드만 삭스 런던사옥 매입 막아주시죠')
('2018-08-03', '기타', '누진세.. 8,9월 한두달만이라도..')
...
```

다른 항목을 설정하면 설정된 항목의 값이 yield 됩니다.

```python
petitions.set_keys('category', 'content')

for category, content in petitions:
    # do something
```
