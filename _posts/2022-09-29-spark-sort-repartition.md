---
title: "[Spark] sort 후 repartition하니 정렬이 풀려요"
author: yoonkij
date: 2022-09-29 09:00:00 +0900
categories: [Dev, Data]
tags: [spark]
render_with_liquid: true
---

### 문제인식
Pyspark로 데이터를 정제하고 두가지 조건을 만족하면서 하둡에 써야했다.
* 3가지 key로 row를 정렬
* 50개 파일로 나누어 출력 제한

잠깐의 구글링으로 각각을 검색 후 아래와 같은 코드로 파일출력을 한 후 확인을 했는데 파일은 50개지만 정렬이 되지 않았다.
```python
# df: Dataframe
df.sort("key1", "key2", "key3").repartition(50)\
  .write.mode('overwrite').option("sep", "\t").option("encoding", "UTF-8")\
  .csv(output_path)
```

### 원인
#### **`repartition`은 `coalesce + shuffle`이다.**
**repartition**
* `repartition`은 전체 데이터를 shuffle 후 원하는 수의 (output) 파티션으로 고르게 데이터를 분할한다.
* 따라서 위 코드처럼 `repartition` 앞에서 `sort`를 했더라도 상관없이 데이터가 섞이고만다.
* 데이터 크기의 불균형이 생겨 정리가 필요할 때, 파티션의 수를 늘릴 때 사용한다.

**coalesce**
* 반면, `coalesce`는 shuffle 옵션을 가진 데이터 분할이다.
* shuffle 옵션을 켜지 않으면 데이터를 섞지 않고 데이터를 정해준 파티션으로 분할한다.
* 파티션의 수를 늘리는 task에 적합하지 않다. (이 경우 shuffle=True해야 하며 repartition과 동일해짐)
* 주로, 데이터 변형이 모두 끝난 후 큰 크기 + 적은 수의 파일로 HDFS에 저장하고자 할 때 사용한다.

#### 해결
```python
# df: Dataframe
df.sort("key1", "key2", "key3").coalesce(50)\
  .write.mode('overwrite').option("sep", "\t").option("encoding", "UTF-8")\
  .csv(output_path)
```

#### 추가 스터디 필요한 부분
* Spark 파티션의 종류와 특징
* 쿼리 최적화와 repartition / coalesce