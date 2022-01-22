# grid
Grid는 flex와 비슷하지만 flexbox는 1차원 레이아웃에 배치하는데 유용, Grid는 2차원 레이아웃에 유용

# Grid Properties
## Grid Container
| 속성 | 의미|
|:----------|:----------|
|display|그리드 컨테이너(Container)를 정의|
|grid-template-rows|명시적 행(Track)의 크기를 정의|
|grid-template-columns|명시적 열(Track)의 크기를 정의|
|grid-template-areas|영역(Area) 이름을 참조해 템플릿 생성|
|grid-template|grid-template-xxx의 단축 속성|
|row-gap(grid-row-gap)|행과 행 사이의 간격(Line)을 정의|
|column-gap(grid-column-gap)|열과 열 사이의 간격(Line)을 정의|
|gap(grid-gap)|xxx-gap의 단축 속성|
|grid-auto-rows|암시적인 행(Track)의 크기를 정의|
|grid-auto-columns|암시적인 열(Track)의 크기를 정의|
|grid-auto-flow|자동 배치 알고리즘 방식을 정의|
|grid|grid-template-xxx과 grid-auto-xxx의 단축 속성|
|align-content|그리드 콘텐츠(Grid Contents)를 수직(열 축) 정렬|
|justify-content|그리드 콘텐츠를 수평(행 축) 정렬|
|place-content|align-content와 justify-content의 단축 속성|
|align-items|그리드 아이템(Items)들을 수직(열 축) 정렬|
|justify-items|그리드 아이템들을 수평(행 축) 정렬|
|place-items|align-items와 justify-items의 단축 속성|
### display: grid;
Grid Container(컨테이너)를 정의합니다.
정의된 컨테이너의 자식 요소들은 자동으로 Grid Items(아이템)로 정의됩니다.


| 값 | 의미|
|:----------|:----------:|
| grid | Block 특성의 Grid Container를 정의 |
| inline-grid | Inline 특성의 Grid Container를 정의|


### grid-template-rows, grid-template-columns
* **grid-template-rows**는 행(row)의 배치
* **grid-template-columns**는 열(column)의 배치
* fr(fraction, 공간 비율) 단위 사용 가능
* repeat() 함수를 사용 가능

```css
// column을 200px, 200px, 500px
grid-template-columns: 200px 200px 500px;

// 1fr 1fr 1fr은 균일하게 1:1:1 비율인 3개의 column을 만든다.
grid-template-columns: 1fr 1fr 1fr;

// 고정크기와 가변크기를 섞어서 사용 가능
grid-template-columns: 100px 2fr 1fr;

.container {
  grid-template-rows: repeat(3, 100px);
  grid-template-columns: repeat(4, 1fr 2fr 3fr);
  /* grid-template-columns: 1fr 2fr 3fr 1fr 2fr 3fr 1fr 2fr 3fr 1fr 2fr 3fr; */
}
```

### grid-template-areas
지정된 그리드 영역 이름(grid-area)을 참조해 그리드 템플릿을 생성합니다.

> ⚠<code>grid-area</code>는 Grid Container가 아닌 Grid Item에 적용하는 속성입니다.

```css
//.(마침표)를 사용하거나 명시적으로 none을 입력해 빈 영역을 정의
.container {
  display: grid;
  grid-template-rows: repeat(4, 100px);
  grid-template-columns: repeat(3, 1fr);
  grid-template-areas:
    "header header header"
    "main . ."
    "main . aside"
    "footer footer footer";
}
header { grid-area: header; }
main   { grid-area: main;   }
aside  { grid-area: aside;  }
footer { grid-area: footer; }
```
![](https://images.velog.io/images/dev_jihee/post/6bbf5b7f-547a-417a-b83b-52eb625b51e5/image.png)

### grid-template
<code>grid-template-rows</code>, <code>grid-template-columns</code> 그리고 <code>grid-template-areas</code>의 단축 속성입니다.
```css
.container {
  grid-template:
    [1행시작선이름] "AREAS" 행너비 [1행끝선이름]
    [2행시작선이름] "AREAS" 행너비 [2행끝선이름]
    / <grid-template-columns>;
}
```

```css
.container {
  display: grid;
  grid-template:
    "header header header" 80px
    "main main aside" 350px
    "footer footer footer" 130px
    / 2fr 100px 1fr;
}
header { grid-area: header; }
main   { grid-area: main; }
aside  { grid-area: aside; }
footer { grid-area: footer; }
```
위 예제의 컨테이너는 다음과 같이 해석할 수 있다.
```css
.container {
  display: grid;
  grid-template-rows: 80px 350px 130px;
  grid-template-columns: 2fr 100px 1fr;
  grid-template-areas:
    "header header header"
    "main main aside"
    "footer footer footer";
}
```

### row-gap, column-gap, gap(간격 만들기)

#### row-gap(grid-row-gap)
각 행과 행 사이의 간격(Gutter)을 지정
> 더 명확하게는 그리드 선(Grid Line)의 크기를 지정한다고 표현할 수 있습니다.

#### column-gap(grid-column-gap)
각 열과 열 사이의 간격(Gutter)을 지정

#### gap(grid-gap)
각 행과 행, 열과 열 사이의 간격(Gutter)을 지정
```css
.container {
  gap: <grid-row-gap> <grid-column-gap>;
}
```
```css
.container {
  display: grid;
  grid-template-rows: repeat(2, 150px);
  grid-template-columns: repeat(3, 1fr);
  gap: 20px 10px;
}
/* 하나의 값으로 통일할 수 있습니다. */
.container {
  gap: 10px;  /* row-gap: 10px; + column-gap: 10px; */
}
/* 하나의 값만 적용하고자 한다면 다음과 같이 사용할 수 있습니다. */
.container {
  gap: 10px 0; /* row-gap */
  gap: 0 10px; /* column-gap */
}

```

![](https://images.velog.io/images/dev_jihee/post/21d795ed-69d0-460c-a7ec-3de955af888c/image.png)

### grid-auto-rows, grid-auto-columns (그리드 형태를 자동으로 정의)

#### grid-auto-rows
암시적 행(Track)의 크기를 정의합니다.
아이템(Item)이 grid-template-rows로 정의한 명시적 행 외부에 배치되는 경우 암시적 행의 크기가 적용됩니다.

```css
.container {
  width: 300px;
  height: 200px;
  display: grid;
  grid-template-rows: 100px 100px; /* 명시적 2개 행 정의 */
  grid-template-columns: 150px 150px; /* 명시적 2개 열 정의 */
  grid-auto-rows: 100px; /* 그 외(암시적) 행의 크기 정의 */
}
.item:nth-child(3) {
  grid-row: 3 / 4;
}
```
![](https://images.velog.io/images/dev_jihee/post/d23e123e-3fdd-4c3a-842d-ad2e1f8f23fd/image.png)

#### grid-auto-columns
암시적 열(Track)의 크기를 정의합니다.
아이템(Item)이 grid-template-columns로 정의한 명시적 열 외부에 배치되는 경우 암시적 열의 크기가 적용됩니다.

```css
.container {
  width: 300px;
  height: 200px;
  display: grid;
  grid-template-rows: 100px 100px;
  grid-template-columns: 150px 150px;
  grid-auto-rows: 100px;
  grid-auto-columns: 100px;
}
.item:nth-child(3) {
  grid-row: 3 / 4;
  grid-column: 3 / 4;
}
```
![](https://images.velog.io/images/dev_jihee/post/c3fe0cfa-a2fd-4d7f-b915-abf2da7cf54e/image.png)


### grid-auto-flow (자동 배치)
배치하지 않은 아이템(Item)을 어떤 방식의 ‘자동 배치 알고리즘’으로 처리할지 정의합니다.

>배치한 아이템은 grid-area(이하 개별 속성 포함)를 사용한 아이템을 의미합니다.

* row: 각 행 축을 따라 차례로 배치
* column: 각 열 축을 따라 차례로 배치	
* row dense(dense): 각 행 축을 따라 차례로 배치, 빈 영역 메움!	
* column dense각 열 축을 따라 차례로 배치, 빈 영역 메움!

```css
/* For column & column dense */
.container {
  display: grid;
  grid-template-rows: repeat(3, 1fr);
  grid-template-columns: repeat(3, 1fr);
  grid-auto-flow: column || column dense;
}
.item:nth-child(1) {
  grid-column: 2 / span 2;
}
.item:nth-child(2) {
  grid-column: span 2;
}
```
![](https://images.velog.io/images/dev_jihee/post/7345901a-3077-4290-8444-56fe990ecfa9/image.png)


### grid
grid-template-xxx과 grid-auto-xxx의 단축 속성입니다.
```css
.container {
  grid: <grid-template>;
  grid: <grid-template-rows> / <grid-auto-flow> <grid-auto-columns>;
  grid: <grid-auto-flow> <grid-auto-rows> / <grid-template-columns>;
}
```

## Grid Items

| 속성 | 의미|
|:----------|:----------|
|grid-row-start|그리드 아이템(Item)의 행 시작 위치 지정|
|grid-row-end|그리드 아이템의 행 끝 위치 지정|
|grid-row|grid-row-xxx의 단축 속성(행 시작/끝 위치)|
|grid-column-start|그리드 아이템의 열 시작 위치 지정|
|grid-column-end|그리드 아이템의 열 끝 위치 지정|
|grid-column|grid-column-xxx의 단축 속성(열 시작/끝 위치)|
|grid-area|영역(Area) 이름을 설정하거나, grid-row와 grid-column의 단축 속성|
|align-self|단일 그리드 아이템을 수직(열 축) 정렬|
|justify-self|단일 그리드 아이템을 수평(행 축) 정렬|
|place-self|align-self와 justify-self의 단축 속성|
|order|그리드 아이템의 배치 순서를 지정|
|z-index|그리드 아이템의 쌓이는 순서를 지정|



### grid-row-start, grid-row-end, grid-column-start, grid-column-end

그리드 아이템(Item)을 배치하기 위해 그리드 선(Line)의 ‘시작 위치’와 ‘끝 위치’를 지정합니다.
‘숫자’를 지정하거나, ‘선 이름’을 지정하거나, span 키워드를 사용합니다.
```css
.item:nth-child(1) {
  /* Row 1번에서 3번(1+2=3)까지 */
  grid-row-start: 1;
  grid-row-end: span 2;

  /* Column 2번에서 3번(2+1=3)까지 */
  grid-column-start: 2;
  /* grid-column-end: span 1; (생략) */
}
```
![](https://images.velog.io/images/dev_jihee/post/6505d69d-cd31-4fe3-a351-eb2aafb1fc89/image.png)
span 키워드를 ‘시작 위치’에 작성하고, ‘끝 위치’를 명시해서 확장할(-) 수도 있습니다.
```css
.item:nth-child(1) {
  /* Column 3번에서 2번(3-1=2)까지 */
  /* grid-row-start: span 1; (생략) */
  grid-row-end: 3;

  /* Column 4번에서 2번(4-2=2)까지 */
  grid-column-start: span 2;
  grid-column-end: 4;
}
```
![](https://images.velog.io/images/dev_jihee/post/be2473a6-ce3c-4861-bb2f-3781228f3564/image.png)

### grid-row
grid-row-start과 grid-row-end의 단축 속성입니다.
각 속성을 /로 구분하는 것에 주의하세요.
```css
.item {
  grid-row: <grid-row-start> / <grid-row-end>;
}

// 각 코드 블록 내 아이템(.item)들은 모두 같은 의미입니다.

.item {
  grid-row-start: 1;
  grid-row-end: 2;
}
.item {
  grid-row: 1 / 2;
}

------------------------
.item {
  grid-row-start: 2;
  grid-row-end: span 3;
}
.item {
  grid-row: 2 / span 3;
}
.item {
  grid-row: 2 / 5;
}
------------------------
.item {
  grid-row-start: span 3;
  grid-row-end: 4;
}
.item {
  grid-row: span 3 / 4;
}
.item {
  grid-row: 1 / 4;
}
------------------------

```

### grid-column
grid-column-start과 grid-column-end의 단축 속성입니다.
각 속성을 /로 구분하는 것에 주의하세요.
```css
.item {
  grid-column: <grid-column-start> / <grid-column-end>;
}
------------------------
.item {
  grid-column-start: -1;
  grid-column-end: -3;
}
.item {
  grid-column: -1 / -3;
}
.item {
  /* Column -1번에서 -3번(-1-2=-3)까지 */
  grid-column: span 2 / -1;
}
------------------------
// 각 코드 블록 내 아이템(.item)들은 모두 같은 의미입니다.
// 음수 결과를 위해 span 키워드를 ‘시작 위치’에 작성함에 주의하세요!

.item {
  grid-column-start: 2;
  grid-column-end: -1;
}
.item {
  /* Column 2번에서 끝(-1번)까지 */
  grid-column: 2 / -1;
}
```

### grid-area
grid-row-start, grid-column-start, grid-row-end 그리고 grid-column-end의 단축 속성입니다.
혹은 grid-template-areas가 참조할 영역(Area) 이름을 설정할 수도 있습니다.
영역 이름을 설정할 경우 grid-row와 grid-column 개념은 무시됩니다.

### align-self (개별 아이템 세로 정렬)
해당 아이템을 세로(column축) 방향으로 정렬합니다. 아이템에 적용합니다.
```css
.item {
	align-self: stretch;
	/* align-self: start; */
	/* align-self: center; */
	/* align-self: end; */
}
```
### justify-self (개별 아이템 가로 정렬)
해당 아이템을 가로(row축) 방향으로 정렬합니다. 아이템에 적용합니다.
```css
.item {
	justify-self: stretch;
	/* justify-self: start; */
	/* justify-self: center; */
	/* justify-self: end; */
}
```

### place-self
align-self와 justify-self를 같이 쓸 수 있는 단축 속성이예요.
align-self, justify-self의 순서로 작성하고, 하나의 값만 쓰면 두 속성 모두에 적용됩니다.
```css
.item {
	place-self: start center;
}
```

### order
그리드 아이템이 자동 배치되는 순서를 변경할 수 있습니다.
숫자가 작을수록 앞서 배치됩니다.

### z-index
z-index 속성을 이용해 아이템이 쌓이는 순서를 변경할 수 있습니다.

```css
.item:nth-child(1) {
  grid-area: 1 / 1 / 2 / 3;
}
.item:nth-child(2) {
  grid-area: 1 / 2 / 3 / 3;
  z-index: 1;
}
.item:nth-child(3) {
  grid-area: 2 / 2 / 3 / 4;
}
```
![](https://images.velog.io/images/dev_jihee/post/466e605a-4e04-4f06-83d7-f593c7fab339/image.png)