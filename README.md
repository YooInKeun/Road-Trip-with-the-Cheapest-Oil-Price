Road Trip with the Cheapest Oil Price
=====================================

Dynamic Graph in Real World(Individual Project)
-----------------------------------------------

<br/>

## 프로젝트 소개

### 주제 : "Road Trip with the Cheapest Oil Price"

### (가장 저렴한 기름값으로, 장거리 자동차 여행하기)

&nbsp;&nbsp;'Road Trip(장거리 자동차 여행)'은 의미상 지은 이름이고, **핵심은 타 지역을 자동차를 이용해 고속도로를 경유하면서 이동할 때 가장 저렴한 기름값으로 이동**하는 것을 목표로한다.

<br/>

## 목차

<!--ts-->

* [구현 시나리오](#구현-시나리오)

* [자료구조](#자료구조)

* [측정가능한 의미 있는 정보](#측정가능한-의미-있는-정보)

* [전제조건](#전제조건)

* [추가 고려사항](#추가-고려사항)

<!--te-->

<br/>

## 구현 시나리오

&nbsp;&nbsp;① 사용자로부터 여행(방문)하고자 하는 여러 지역을 '일자별로' 선택받는다. 이때 여행기간에는 제한이 있으며, 출발지 및 여행(방문) 지역은 서울, 대전, 대구, 인천, 부산, 울산, 광주, 창원 등의 대도시로 선정된다.

```
ex) 사용자가 다음과 같이 입력

여행 기간 : 9월 1일 ~ 9월 4일(3박 4일)

- 9월 1일(1일차) : 서울 -> 대구

- 9월 2일(2일차) : 대구 -> 울산
 
- 9월 3일(3일차) : 울산 -> 부산 -> 창원

- 9월 4일(4일차) : 창원 -> 대전 -> 서울(출발지로 돌아옴)
```

<br/>

&nbsp;&nbsp;② 일자별 이동 고속도로를 안내하고, 고속도로 동선상에 있는 '**주유소(Node)**' 및 '**기름값(Weight_1)**' 그리고 '**넣어야 하는 기름의 양(Weight_2)**'을 계산한다. (데이터는 네이버 지도, 카카오 네비, 한국석유공사 Opinet 등에서 추출)

```
- 9월 1일(1일차) : 서울 -> 대구

=> '경부고속도로', 동선상의 주유소(Node), 기름값(Weight_1), 기름의 양(Weight_2) 안내


- 9월 2일(2일차) : 대구 -> 울산

=> '중앙고속도로', 동선상의 주유소(Node), 기름값(Weight_1), 기름의 양(Weight_2) 안내


- 9월 3일(3일차) : 울산 -> 부산 -> 창원

=> '경부고속도로', '남해제2고속도로지선', 동선상의 주유소(Node), 기름값(Weight_1), 기름의 양(Weight_2) 안내


- 9월 4일(4일차) : 창원 -> 대전 -> 서울

=> '중부내륙고속도로', '경부고속도로', 동선상의 주유소(Node), 기름값(Weight_1), 기름의 양(Weight_2) 안내
```

<br/>

&nbsp;&nbsp;③ 여행(방문)기간 동안 가장 저렴하게 기름값을 지불할 수 있는 '**주유소(Node)**'의 경로 및 '**기름값(Weight_1)**', '**주유할 기름의 양(Weight_2)**'을 안내한다.

```
(Note) 이때 '주유할 기름의 양(Weight_2)'을 나타내야 하는 이유는 다음과 같다.
```

&nbsp;&nbsp;사용자는 서울에서 부산으로 이동하고, 경부고속도로 상에서 가장 저렴한 주유소가 A라고 가정하자. 이때 주유소 A까지 도달하기 까지 자동차에 기름이 충분치 않아 그 전에 기름을 넣어야만 하는 경우, **자동차가 도달할 수 있는 가장 저렴한 주유소에서 기름을 A까지 갈 수 있는 만큼만 조금 넣고, A로 가서 기름을 가득 넣을 수도 있다. 즉, 하나의 고속도로 상에서 두 개 이상의 주유소를 이용하는 것이 어떤 하나의 주유소에서 기름을 넣는 것보다 기름값이 저렴하다면, 두 개 이상의 주유소를 이용하는 것을 '주유할 기름의 양(Weight_2)'과 더불어 안내**한다.

<br/>

&nbsp;&nbsp;④ 여행 기간 동안에 가장 저렴한 기름값으로 총 얼마가 나왔고, 가장 비싼 기름값 혹은 평균적인 기름값과 비교해서 얼마나 차이 나는지를 보여준다.

<br/>

## 자료구조

```
- Node : 고속도로 동선상에 있는 도시 및 주유소(도시는 Weight가 없는 Node)

- Edge : 고속도로 길

- Weight(2개) : Node(주유소)의 기름값(Weight_1), 주유할 기름의 양(Weight_2)
```

&nbsp;&nbsp;이때 주유소의 기름값은 하루마다 갱신되므로, 'Weight_1'의 값은 하루 단위로 바뀌기 때문에 Dynamic한 Graph로 표현할 수 있다. 이때 한 주유소에서 기름값이 변화하는 폭이 크지 않고, 기름값이 오르거나 내리는 시기가 대부분의 주유소가 비슷하다. 하지만 기간에 따라 다른 주유소의 기름값이 변하는데 반해, 기름값이 거의 일정한 주유소들도 있다. 그리고 기간이 길다면 기름값의 차이가 점점 커지는 것을 알 수 있다. 그래서 여행 기간이 길수록 의미 있는 값을 측정할 수 있다. 또한, 'The Cheapest Oil Price'를 찾는 것에 큰 의미가 있다.

<br/>

## 측정가능한 의미 있는 정보

```
- Road Trip with the Cheapest Oil Price : 장거리 여행 동안에 가장 저렴한 기름값을 측정

- Community : 주변 경쟁 주유소들 간의 관계 및 영향

- Centrality : 기름값이 가장 저렴한 주유소
```

<br/>

## 전제조건

&nbsp;&nbsp;① 한국석유공사(Opinet)에 등록된 고속도로 상에 있는 주유소만을 이용(고속도로를 벗어난 지역은 Data가 너무 많아 고려하기 힘들다)

<br/>

&nbsp;&nbsp;② 고속도로 선택 시, 가장 짧은 거리만을 기준으로 삼는다.

<br/>

&nbsp;&nbsp;③ 하루 동안에 방문 지역이 많아 실제 24시간이 넘어도, 각 방문 지역에서 소비되는 시간은 계산 불가하므로 이동 시간이 24시간이 넘든 넘지 않든 사용자가 입력한 대로 프로그램을 수행한다.

<br/>

## 추가 고려사항

&nbsp;&nbsp;① 방문 도시는 몇 개 까지 표현할 것인가

<br/>

&nbsp;&nbsp;② 차종에 따라 기름 종류, 연비가 다른데 어디까지 세분화해서 구현할 것인가

<br/>

&nbsp;&nbsp;③ 고속도로에서 동선상에 있는 주유소만을 어떻게 검색할 것인가

<br/>

&nbsp;&nbsp;ex) ‘대전 -> 대구’는 경부고속도로의 일부!

<br/>

&nbsp;&nbsp;‘경부고속도로’ 전체에는 많은 주유소들이 존재한다. 하지만 알아본 바로 Opinet 상에는 주유소 위치를 고속도로별로만 알려준다. 고속도로 상에 있는 두 도시 사이의 주유소를 따로 알려주지 않는다.
