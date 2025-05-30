# Mapping 
- Elasticsearch에서 각 필드의 데이터 타입, 색인 여부, 분석기 설정 등 문서가 저장되고 검색되는 방식을 정의하는 속성들의 집합이다. 
- Elasticsearch는 각 필드를 어떻게 처리할지 결정하고, 효율적인 검색과 저장을 가능하게 한다.

## Index
- 인덱스는 문서를 저장할 때, 각 필드는 검색이 가능해야 할지(Searchable)를 결정할 수 있다.

### `index : false`
- `index: false`로 설정하면, 해당 필드는 저장만 되고 검색은 불가능하다.
- 검색용 inverted index를 안 만들기 때문에 디스크 사용량 줄어 공간을 절약할 수 있다.
-  검색용 index를 안 만들면 성능(`indexing 속도 ⬆️, throughput ⬆️`)이 향상된다.
- 로그 데이터, 센서 데이터 등은 대부분 시간 순서대로 저장하고, 분석은 전체 문서 기준으로 하기 때문에, time series처럼 많은 데이터를 빠르게 저장해야 하는 경우 `index: false`를 사용하는 것이 유용하다.

## format
- 날짜 필드(type: date)에 대해 "이런 형식으로 날짜가 들어올 거야" 하고 미리 알려주는 옵션이다.
- format을 지정하면 정확도 향상, 에러방지, 성능 향상, 데이터 일관성 유지와 같은 효과를 볼 수 있다.

## coerce
- Elasticsearch가 데이터 타입을 자동 변환하려고 시도하게 해서, 입력 유연성을 높여주는 옵션이다.
- 🔥 예상치 못한 형변환이 일어날 수 있으니, 데이터 정합성이 중요한 경우엔 false로 두는 게 안전하다.

## doc_values
- `doc_values: true`는 이 필드에 대해 정렬하거나 집계할 수 있도록, 디스크에 컬럼 기반 구조로 저장한다는 의미이다.
- 텍스트는 정렬/집계할 일이 거의 없고, 용량만 많이 차지하기 때문에 text 필드엔 기본적으로 `false`이다.

## norms
- Elasticsearch에서 문서의 중요도를 결정하는 데 사용하는 정보(문서의 길이, 필드에 부여된 boost 값)를 저장하는 옵션이다.
- 검색 결과의 relevance score (즉, 점수) 를 계산할 때 더 정교한 판단을 할 수 있다.

## null_value
- 필드가 null일 때, 인덱싱 시 대신 저장할 대체값을 지정하는 옵션이다.

### null_value를 사용하는 경우?
- 값이 비어 있더라도 필드 존재 여부를 추적하고 싶을 때
- null 값도 일종의 "의미 있는 값"으로 다뤄야 할 때
- 통계/집계 시 null을 무시하지 않고 처리하고 싶을 때

## copy_to
- 하나 이상의 필드에 입력된 값을 다른 필드에 자동으로 복사해서, 그 필드를 통해 통합 검색이 가능하게 만드는 매핑 옵션이다.
- `first_name`, `last_name` -> `full_name`

