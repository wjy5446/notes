# Geopandas

## Visualize

- 데이터에서 `Shapely` 패키지를 사용하여 시각화 (**Polygon, LineString, Point**)
- `GeoSeries`, `GeoDataFrame` 의 `plot()` 을 사용



### plot

- 카테고리 시각화 : `counties.plot(figsize=(15,15), column='continent', categorical=True)`
- 실수값 시각화 : `countries.plot(figsize=(15,15), column='gdp_per_cap', scheme='quantiles')`
  - Quantiles : 4등분위
  - Equal_interval : 동일한 간격
  - Fisher_Jenks : 클래스 내 분산을 줄이고, 클래스끼리 분산을 최대화하는 방법



## Geometic data

### Polygons

- 위치를 다각형(여러개의 점)으로 표현한 객체

### Points

- 위치를 점으로 표현

### LineString

- 경계면만 추출



### 속성

- Area : 넓이
- Boundary : 테두리
- Centroid : 중앙지점
- distance() : 거리 계산



## 관계 연산

- 지리데이터 간에 연산을 해주는 기능



- `within`, `contains` : 지리적으로 포함되는 지 여부

  ```
  seoul.within(korea)
  ```

- `intersects`,  `crosses` : 지리적으로 교차 여부 파악

      ```
china.intersects(korea)
      ```



### 서울 특별시 지도 load

- 서울시 지도 다운로드 (http://www.juso.go.kr/addrlink/addressBuildDevNew.do?menu=bsin)

- shape file : .shp, .dbf, .shx 로 이루어진 데이터
  - `.shp` : 점, 다각형의 지리정보를 저장
  - `.shx` : 지리정보의 인덱스 정보 저장
  - `.dbf` : 지리정보의 속성 정보 저장
  - `GRS80_UTMK.prj` : 좌표계 정보
  - `.shp` 는 `.dbf`, `.shx` 가 없으면 불러오기가 힘듬



- load data

```
path_data = '*.shp'
seoul = gpd.read_file(path_data, encoding='euckr')

# plot
seoul.plot(figsize=(11, 11))
```



- 지리정보 변환 함수
  - `convex_hull()` : Polygon 데이터의 convec hull을 생성
  - `envelope()` : Polygon을 감싸는 제일 작은 사각형 생성
  - `simplify(tolerance, preserve_topology=True)` : Polygon의 컨투어를 추정
  - `buffer(distance, resolution=16)` : 
    - Point, LineString에 적용시 주어진 거리내 모든 점을 이어서 Polygon 생성
    - Polygon에 적용시 주어진 거리만큼 혹장
  - `unary_union()` : 여러개의 geometry 데이터의 합집합을 구함,
    - 빈곳이 존재시 실행되지 않음
  - `dissolve(by=)` : group by 처럼, geometric의 그룹별로 unary_union 처리



### CRS (Coordinate reference systems)

- 지구 표면을 2차원 평면에 표현하는 방법

- `.crs`: 좌표계 확인



- 좌표의 종류
  - `WGS84` : GPS가 사용하는 좌표계
  - `Bessel 1841` : 한국과 일본에 잘 맞는 지역 타원체를 사용한 좌표계
  - `Web mercator projection` : 구글/빙/야후/OSM 에서 사용하는 좌표계
  - `Albers projection` : 미국 지질 조사국에서 사용하는 좌표계



- `.to_crs()` : 좌표계 변환