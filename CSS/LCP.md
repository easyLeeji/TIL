

# 가장 큰 콘텐츠 그리기
![](https://images.velog.io/images/dev_jihee/post/b48d2c87-4ed7-4899-bad0-0678c4356c05/image.png)
Lighthouse를 통해서 LCP를 측정한 화면


## Core Web Vitals
![](https://images.velog.io/images/dev_jihee/post/cae17eec-82a6-456f-bf83-17da768ab116/image.png)
여러가지 도구가 있지만 Lighthouse를 추천!

## LCP 개선 사례
### 라이브러리 의존 줄이기
**As is**
```html
<head>
  <script src="jquery-3.6.0.js"></script>😱
</head>
```
![](https://images.velog.io/images/dev_jihee/post/02ba59e7-b3d0-4934-ba1b-416b27819f2c/image.png)
❗ 제이쿼리 라이브러리가 다 로딩된 이후에야 "FCP"(First contentful paint)가 발생
가능하면 제이쿼리 제거하는 것을 추천!

---

#### You might not need*.

[https://youmightnotneed.com](https://youmightnotneed.com)👍
-> jQuery나 Lodash 대신 사용할 수 있는 '바닐라스크립트'를 찾아 볼 수 있음.

---

### Remove unused CSS
```html
<head>
	<link rel="stylesheet" href="normalize.css">😱
</head>
```
'normalize'나 'reset'과 같이 잘 사용하지 않는 css는 과감하게 버리기!

---

### 웹 폰트 Preconnect / CSS Preload
```html
<head>
	<link rel="stylesheet" href="*.css">😱
</head>
```
-> CSS를 로딩하고 해석하는 동안 웹페이지의 렌더링이 차단 된다.

```html
<head>👏👏👏
	<link rel="preconnect" href="https://fonts.gstatic.com">
	<link rel="preload" as="style" href="*.css" onload="this.onload=null;this.rel='stylesheet;">
</head>
```
-> preconnect : 미리 연결, preload: 웹 페이지 렌더링을 하는 동시에 CSS 파일을 다운로드하게 되어 있다. onload 스크립트를 이용해서 CSS 다운로딩이 끝나면 preload 값을 'stylesheet'로 바꿔 주어 웹페이지에 적용시켜준다(**지연 적용**)

#### Preconnect / Preload
```
<link rel="preconnect">
```
* 도메인을 알지만 자원의 최종 경로를 모르는 경우 서버와의 연결을 미리 설정.
* DNS(Domain Name Server), TCP(Transmission Control Protocol), TLS(Transfer Layer Security) 왕복에 필요한 시간을 단축.
* 서드 파티 자원 연결에 적합. 
(Preconnect를 구글 웹 폰트 참조하고 로드하는데 사용 추천)

```
<link rel="Preload">
```
* 필요한 자원을 병렬 다운로드.
* 자원을 로딩하는 동안 렌더링을 차단하지 않음.
* as 속성을 함께 명시해 주어야 한다.
	예) as="style", as="script", as="image".
    
    ---
    
### 이미지 preload

```html
<head>👏👏👏
	<link rel="preload" as="image" media="(max-width:640px)" href="small.avif">
	<link rel="preload" as="image" media="(min-width:641px)" href="large.avif">
</head>
```
-----

### Feature detection (image type/Viewport width)
**As is**
```html
<!-- UNOPTIMIZED IMAGE -->
<img src="large.jpg" alt>😱
```


**To be**
```html
<!-- OPTIMIZED IMAGE -->
<picture>
  <!-- AVIF && SMALL SCREEN -->
  <source srcset="small.avif" type="image/avif" media="(max-width:640px)">
  <!-- AVIF && LARGE SCREEN -->
  <source srcset="large.avif" type="image/avif">
  <!-- WEBP && SMALL SCREEN -->
  <source srcset="small.webp" type="image/webp" media="(max-width:640px)">
  <!-- WEBP && LARGE SCREEN -->
  <source srcset="large.webp" type="image/webp">
  <!-- FALLBACK -->
  <img src="small.jpg" alt>
</picture>
👏👏👏
```
-----

### Image Loading / Decoding

```html
<!-- UNOPTIMIZED IMAGE -->
<img src="large.jpg" alt>😱
```

```html
<!-- OPTIMIZED IMAGE -->
<img src="example.jpg" 
     loading="lazy"👏
     decoding="async"👏
    alt>
```
* 로딩 이미지 지연 로딩 기법(lazy): 사용자가 스크롤하면서 페이지 문서가 뷰 포트 화면 안쪽으로 들어올 때 모니터 뷰 포트의 약 1배~2배 크기 아래쪽부터 이미지 로딩이 시작 된다 
* Decoding: 암호화했던 이미지를 복호화하는 방법으로 'async'를 제공하면 화면에 다른 요소를 렌더링하는 걸 중단하지 않고 다른 요소를 먼저 표시하고 이미지를 뒤늦게 화면에 표시하는 지연 가능한 기법


```
<img decoding="lazy">
```
> 뷰포트 밖에 있는 이미지를 로딩하지 않는다.
대략 뷰포트 높이의 1~2배 지점까지 근접하면 로딩됨.

```
<img decoding="async">
```
> 이미지 디코딩(복호화)을 병렬 처리.
디코딩을 지연시켜 다른 콘텐츠의 표시 속도가 빨라짐


---

# Summary
1. LCP는 뷰포트에 표시하는 가장 큰 콘텐츠 렌더링 성능.
2. 가장 큰 덩어리 콘텐츠를 2.5초 이내로 표시해야 한다.
3. JS/CSS 라이브러리 의존도를 낮추어야 한다.
4. preconnect/preload 속성으로 외부 자원 최적화.
5. photo 요소의 type, media 속성으로 이미지 전송량 최적화.
6. loading/decoding 속성으로 이미지 렌더링 최적화.