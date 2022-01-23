# CSS flex
>플렉스는 박스의 **크기, 방향, 순서, 정렬, 간격을** 제어하는 새로운 박스 모델

[https://www.w3.org/TR/css-flexbox-1](https://www.w3.org/TR/css-flexbox-1)

## CSS flex 'IE' bugs😱
![](https://images.velog.io/images/dev_jihee/post/07e54ef7-de98-4815-a6b6-f1e0705ce389/image.png)

## 아이템 크기 자동 분배(flex-grow/-shirink/-basis)
![](https://images.velog.io/images/dev_jihee/post/a5c0b2e4-9541-4182-9a08-12d1e3d2a2fa/image.png)

## 진행 축 정렬(공간 자동 분배, justify-content)
박스와 박스 사이에 여백을 예전에는 'Auto margin'이 이 공간을 형성했는데 'flexbox'는 'Free space'로 처리된다.

flex-direction: column; 처리하고 justify-content: center; 처리하면 항상 수직, 가운데 정렬

## 교차 축 정렬 (공간 자동 분배, align-items/ -self, -content)

* align-items: 한 줄짜리 교차 축 아이템 정렬
* align-self: 하나만 골라서 정렬
* align-content: 여러 줄 일때 교차 축 정렬

## 배치 순서 변경(order)

## 감싸기 (flex-direction/ -wrap/ -flow)

-------------

## Flex 용어
1. flex container(플렉스 컨테이너)
2. flex item(플렉스 아이템)
3. free space(빈 공간)
4. main axis(진행 축)
5. cross axis(교차 축)

### flex container
> 'display' 속성 값이 'flex' 또는 'inline-flex'인 박스
내부에 흐르는 자식 콘텐츠(엘리먼트, 텍스트 노드)는 저절로 플렉스 아이템이 된다.

플렉스 컨테이너에만 적용 가능한 속성이 있다.
```css
.flex-container {
	display: flex; /* 컨테이너 선언 */
    flex-flow: row nowrap; /* 흐름방향 + 줄 바꿈 */
    flex-direction: row; /* 흐름 방향 */
    flex-wrap: nowrap; /* 줄 바꿈 */
    justify-content: flex-start; /* 진행 축 정렬 */
    align-items: stretch; /* 교차 축 정렬 */
    align-content: stretch; /* 여러 줄 교차 축 정렬 */
}
```

### flex item
> 플렉스 컨테이너 내부에서 흐르는 자식 박스
(엘리먼트, 텍스트 노드)를 플렉스 아이템이라고 부른다.
플렉스 박스 모델에 따라 배치된다.

플렉스 아이템에만 적용 가능한 속성이 있다.
```css
.flex-container {
	flex: 0 1 auto; /* 팽창지수 + 수축지수 + 기준 크기 */ /* flex-grow, -shirnk, -basis 속성의 단축 속성 */
    flex-grow: 0; /* 팽창지수 */
    flex-shrink: 0; /* 수축지수 */
    flex-basis: auto; /* 기준 크기 */
    align-selft: auto; /* 독립적 교차 축 정렬 */
    order: 0; /* 배치 순서 */ 
}
```

## Free space⭐
> 플렉스 아이템이 점유하는 영역 (flex-basis, width, height, padding, border, margin)을 제외하고 남은 공간을 프리 스페이스라고 부른다.
'0, 양수, 음수' 프리 스페이스가 발생할 수 있다.
프리 스페이스는 플렉스 아이템의 팽창 지수(flex-grow)와 수축 지수(flex-shirink)를 이용하여 플렉스 아이템으로 분배할 수 있다.

## Main axis(진행 축)⭐
>플렉스 아이템 배치 방향으로써 플렉스 컨테이너에 flex-direction 속성을 적용하여 설정할 수 있다.
진행 축(flex-direction)의 초기 값은 row이며 row-reverse, column, column-reverse 값을 적용할 수 있다.
(row-reverse, column-reverse 으로 값을 뒤집는 상황은 추천하지 않음)

## Cross axis(교차 축)⭐
>플렉스 아이템 배치 방향과 직각으로 교차하는 방향으로써 flex-direction 값의 직각 방향.
진행 축의 방향에 따라 교차 축이 달라지기 때문에 교차 축은 행이 될 수도 있고 열이 될 수도 있다.
align-* 속성의 기준이 되는 축이다.

--------------

## 플렉스 아이템의 팽창과 수축
```css
	flex-grow: 0; /* 팽창지수 */
	flex-shrink: 0; /* 수축지수 */
	flex-basis: auto; /* 기준 크기 */
    flex: 0 1 auto; 👍    
```

### flex-grow:
> 양의 Free space 발생 시 플렉스 아이템의 팽창을 제어

초기 값
* 값: <number>
  //음수 사용 불가. 보통 '0'또는 '1'사용.
* 초기 값: 0
  // 단축 속성에서 생략하면 '1'이 됨.
* 적용: 플렉스 아이템
  
```css
.item { flex-grow: 0;}
 // 플렉스 컨테이너에 양의  Free space가 발생했지만 
  초기 값은 '0'으로써 **팽창하지 않는다.**
```
  
```css
.item { flex-grow: 1;}
 // 양의 Free space 발생 시 'flex-grow:1'을 적용하면
  플렉스 아이템은 1:1 비율로 균등 팽창.
```
  
```css
.item1 { flex-grow: 1;} .item2 { flex-grow: 2;}
 // 플렉스 아이템마다 팽창 비율을 다르게 처리할 수 있다. 1:2 비율로 팽창
```
  
### flex-shrink:
> 음의 Free space 발생 시 플렉스 아이템의 수축을 제어
  
초기 값
* 값: <number>
  //음수 사용 불가. 보통 '0'또는 '1'사용.
* 초기 값: 1
  // 단축 속성에서 생략해도 '1'이 됨.
* 적용: 플렉스 아이템
  
```css
.item { flex-shrink: 0;}
 // 음의 Free space가 발생했다. 'flex-shrink: 0'을 적용하면
  음의 Free space가 발생하더라도 **수축하지 않는다.**
```
```css
.item { flex-shrink: 1;}
 // 초기 값은 '1'으로써 음의 Free space 발생 시 **균등 수축.**
  1:1 비율로 수축한 결과.
```
  
```css
.item1 { flex-shrink: 1;} .item2 { flex-shrink: 2;}
 // 플렉스 아이템마다 수축 비율을 다르게 처리할 수 있다. 1:2 비율로 수축
```
  
### flex-basis:
> 플렉스 아이템의 진행 방향 기본 크기를 설정함으로써 Free space 초기 값에 영향을 준다.
  
  * 값: content ⚠ | <code>**<'width'>**</code>
  * 초기 값: **auto**
  //content 또는 <code><'width'></code> 값이 적용됨.
  //단축 속성에서 생략하면 초기 값이 '0'이 된다.
  * 적용: **플렉스 아이템**
  
```css
.item { flex-basis: auto;}
 // 플렉스 아이템에 적용된 **기본 크기.**
  컨테이너에 남은 공간이 양의 Free space가 된다.
```
```css
.item { flex-basis: 30%;}
 // 진행 축으로 30% 크기가 아이템의 **기본 크기.**
  컨테이너에 남은 공간(40%)이 양의 Free space 가 된다.
```
```css
.item { flex-basis: 60%;}
 // 진행 축으로 60% 크기가 아이템의 기본 크기.
  컨테이너에 초과하는 공간(-20%)이 음의 Free space 가 된다.
```
```css
.item { flex-basis: 0;}⭐
 // 아이템의 기본 크기가 '0'이므로 컨테이너의 100%가 양의 Free space가 되었다.
```
  
### flex:  ⭐
  > 플렉스 아이템의 '팽창, 수축, 기본크기'를 제어하는 단축 속성.
  
* 값: none|[<'flex-grow'><'flex-shrink'>?||<'flex-basis'>]
  //[반드시 하나,?생략하거나 한번, || 하나 또는 그 이상.
* 초기 값: 0 1 auto
  // 생략한 속성의 값은 재설정(변경)된다.
* 적용: 플렉스 아이템
  
#### flex: 1 ⭐ 실무에서 사용할 확률 99%
  
```css
.item {flex:1}✨
  
  ==
  
.item {flex:1 1}
  
  ==

.item {flex:1 1 0}  
```
  flex-grow 또는 flex-shrink 값을 선언하면 flex-basis값은 'auto'에서 '0'으로 변경된다.
  
  
---------
## 플렉스 아이템의 방향과 순서
```css
flex-direction: **row** | row-reverse | column | column-reverse;
flex-wrap: **nowrap** | wrap | wrap-reverse;
flex-flow: <'flex-direction'> || <'flex-wrap'>;⭐
order: **<integer>**;//0, 1, -1...
```
### flex-shrink:
플렉스 아이템의 진행 방향:
 * 값: **row** | row-reverse | column | column-reverse;
* 초기 값: row
* 적용: 플렉스 컨테이너
  
### flex-wrap:
* 값:  **nowrap** | wrap | wrap-reverse;
* 초기 값: nowrap
* 적용: 플렉스 컨테이너
  
### flex-flow:⭐
플렉스 아이템의**진행 방향**과 **줄 바꿈** 단축.
 * 값: <'flex-direction'> || <'flex-wrap'>;
* 초기 값: row nowrap
* 적용: 플렉스 컨테이너
  
### order:⭐
플렉스 아이템의 배치 순서
* 값: <code><'integer'></code>;
  // '0, 양의 정수, 음의 정수'
* 초기 값: 0
* 적용: 플렉스 컨테이너
 
-----
## 플렉스 아이템의 정렬과 간격
### justify-content:⭐
플렉스 아이템 진행 축 정렬
 * 값: flex-start | flex-end | **center | space-between** | space-around | space-evenly
* 초기 값: flex-start
* 적용: 플렉스 컨테이너

![](https://images.velog.io/images/dev_jihee/post/10fa94f3-f86b-4c2f-a081-c5a99823b27d/image.png)
  
### align-itmes:⭐
플렉스 아이템이 한 줄일 때 교차 축 정렬.
 * 값: flex-start | flex-end | **center ** | baseline | **stretch**
* 초기 값: stretch 
  // 아이템을 교차 축으로 잡아 늘림.
* 적용: 플렉스 컨테이너

![](https://images.velog.io/images/dev_jihee/post/f6b40404-ffd2-478e-9c44-8e4af79eeffe/image.png)
  

### align-self:
플렉스 아이템의 독립적 교차 축 정렬.
 * 값: auto | flex-start | flex-end | center | baseline | stretch
* 초기 값: auto 
  // 컨테이너의 align-items 값을 상속 받음.
* ** 적용: 플렉스 아이템**

![](https://images.velog.io/images/dev_jihee/post/7e965b53-0f5a-4d67-9807-85953397f4aa/image.png)
  
### align-content:
플렉스 아이템의 독립적 교차 축 정렬.
 * 값: stretch | **center** | flex-start | flex-end | space-around | **space-between** | space-evenly
* 초기 값: stretch 
  // 줄 간격을 교차 축으로 균등하게 늘림.
* ** 적용: 여러 줄 플렉스 컨테이너

![](https://images.velog.io/images/dev_jihee/post/f6b40404-ffd2-478e-9c44-8e4af79eeffe/image.png)
  
  
  
### gap:⭐
다중 컬럼, 플렉스, 그리드 아이템 사이의 간격.
 * 값: <code><'row-gap'></code><code><'column-gap'></code> ?
* 초기 값: normal 
  // == 다중 컬럼에서 1em 그렇지 않으면 0
* 적용: 컬럼/플렉스/그리드 컨테이너
  
  
 ## Summary
 ❗ flex-grow/-shrink/-basis 대신 **flex** 단축 속성 사용을 권장. **flex** 단축 속성을 통해 팽창, 수축, 기본 크기를 이해하고 제어하느 것이 핵심.
 ❗ **justify-content, align-items** 속성을 통해 정렬 방식을 이해하고 align-self/-content, flex-direction/-wrap/-flow, order 속성은 필요할 때 학습.
❗ **진행 축**을 뒤집는(-reverse)것을 추천하지 않음.
  
  
[flexbox 참고 url](https://flexbox.help)
