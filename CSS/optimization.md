# CSS Optimization

> Remove unused CSS.
사용하지 않는 CSS 제거.
> Eliminate render-blocking resources.
렌더 차단 리소스 제거.


## Remove unused CSS
> Unused CSS 왜 문제인가?
CSS는 페이지 렌더링을 차단하는 리소스 브라우저가 스타일을 계산하는데 잠재적으로 더 많은 시간을 소비
구글 라이트하우스는 2KB 이상 미사용 CSS가 포함된 파일을 검출.

## Goolge Lighthouse - Performance
![](https://images.velog.io/images/dev_jihee/post/8cca8a33-5254-488a-bdc7-f0b645b4443b/image.png)

generate report를 눌러서 퍼포먼스 체크를 할 수 있다.
그러면 **Remove unused CSS**와 같은 항목을 확인 할 수 있다.

![](https://images.velog.io/images/dev_jihee/post/0a683ba1-c29e-4f00-b0bb-74e22b17d81e/image.png)

이 항목을 클릭하면 좀 더 자세한 내용을 확인 가능 -> Coverage Tab을 누르면 실제로 사용되지 않는 CSS 코드를 확인할 수 있다.

⚠ 소스 내에 font-face를 사용하지 않는 CSS라고 무조건 보고하는 오류가 있다.


커버리지 탭 확인하기 commnd+shift+p
![](https://images.velog.io/images/dev_jihee/post/d3d11bfa-5008-4a13-a3a3-e290f551479b/image.png)
![](https://images.velog.io/images/dev_jihee/post/98bc8493-07f7-4813-bc33-486cfce15157/image.png)
소스의 붉은색 영영이 사용하지 않는 코드



-------------------


## Render-blocking resources

> Render blocking 왜 문제인가?
브라우저가 외부 리소스를 다운로드하고 파싱하는 동안 페이지 콘텐츠를 파싱하거나 렌더링하지 않기 때문에 페이지 표시 속도 저하의 원인.
Unused CSS는 Render blocking을 가중하는 요인.


###  Render-blocking 이슈 해결하기!
![](https://images.velog.io/images/dev_jihee/post/8cca8a33-5254-488a-bdc7-f0b645b4443b/image.png)

구글 lighthouse -> Performance -> 검사 항목 중에 Eliminate render-blocking resources 

![](https://images.velog.io/images/dev_jihee/post/f9033d63-8bb4-4263-84f7-b638667e0cf0/image.png)

👉브라우저가 다운로드하고 해석하는 동안 웹 페이지에는 아무것도 보이지 않기때문에 이런 요인을 제거해야한다고 경고로 알려주고 있다.


### Render blocking resources
> 렌더 블로킹 리소스 표시 조건:
* defer, async 속성이 없는 <'head'> 요소의 <'script'>태그.
* media 속성과 값이 없는 <'link rel="stylesheet"'> 태그
  
 #### Render blocking <code><'script'></code>
  ⚠ Render blocking resources를 제거하려면 <code><'script'></code> defer, async 속성을 사용해야하고 stylesheet 태그에는 media 속성을 사용해야 오류를 피할 수 있다.

[Article](https://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html)
![](https://images.velog.io/images/dev_jihee/post/e19d4aa6-7a43-4d20-9f38-1ae1e211556e/image.png)

  ** 1. No async. No defer**

![](https://images.velog.io/images/dev_jihee/post/2875d5fd-7b92-4a1b-9ccf-f6f78ed2f0c4/image.png)


  
  ** 2. async : 병렬 다운로드, 즉시 실행 **  

  ![](https://images.velog.io/images/dev_jihee/post/0c9b470d-1507-4bff-8a2d-2e345c166f7c/image.png)


  
  
  ** 3. defer : 병렬 다운로드, 지연 실행 **  

  ![](https://images.velog.io/images/dev_jihee/post/e370f695-bc03-4d2e-9a57-33500be4e1e1/image.png)
  


 ⚠ 첫번째와 두번째는 HTML 파싱이 중간에 끊어졌다가 다시 이어지는데 script의 defer 속성을 사용하면 HTML 파싱이 중단 없이 계속 진행되게 된다. 따라서 HTML은 빠르게 파싱이 되고 화면에 빠르게 내용물을 그려내게 된다.
  
```script
//병렬 다운로드, 즉시 실행
  <script async src="script.js"></>
//병렬 다운로드, 지연 실행
  <script defer src="script.js"></script>
```
  
 👉 defer은 웹 페이지가 모두 그려지고 'DOM'이 들어왔을 때 그때 스크립트를 실행! 가장 추천하는 옵션⭐
  
  1. 필수 스크립트는 html에 <code><'script'></code>형식으로 작성.
  2. 기타 스크립트는 <'/body'> 종료 태그 직전에 선언.
  3. 마지막에 파싱해도 문제 없으면 defer 속성.
  4. 가능한 빠른 시점에 실행 필요하면 async 속성.

#### Render blocking  <'link rel="stylesheet"'> 
media 속성이 없거나 값이 all이면 렌더 차단 리소스.
```html
<!-- Render blocking resource -->
  <link href="style.css" rel="stylesheet">
  <link href="style.css" rel="stylesheet" media="all">
<!-- Render blocking resource --> 
//위의 소스는 media속성이 없기 떄문에 스타일시트를 다운로드하고 해석하는 동안 웹 브라우저 화면에는 아무것도 그려 내지 않는다. 렌더 차단🚫
    
  <link href="portrait.css" rel="stylesheet" media="orientation:portrait">
  <link href="print.css" rel="stylesheet" media="print">    
    //위의 소스는 미디어 속성이 들어가 있다
      첫번째 코드는 orientation:portrait일때만 스타일시트를 해석하는 조건이고 
      두번째 코드는 print일때만 스타일 시트를 해석하는 조건.
      특정 조건에서만 CSS를 해석하도록 조건문 처리를 했음
```
 [https://t.ly/GiGe](https://t.ly/GiGe)
 [https://is.gd/2jWuot](https://is.gd/2jWuot)
    
CSS 파일이 렌더링을 차단하는 과정    
![CSS 파일이 렌더링을 차단하는 과정]( https://web-dev.imgix.net/image/admin/WhpaDYb98Rf03JmuPenp.png?auto=format&w=845)
  
##### 해결 하는 방법
*  방법 1) 반응형 웹인 경우 해상도 구간별로 CSS 파일을 분리하고 media 속성으로 분기.
  
  ```html
   <link href="*.css" rel="stylesheet" media="(max-width: 639px)">
   <link href="*.css" rel="stylesheet" media="(min-width: 640px) and (max-width:960px)">
   <link href="*.css" rel="stylesheet" media="(min-width: 961px)">
  ```
  
* 방법 2)
  1. 필수 스타일은 페이지 <'head'>에 <'style'>형식으로 작성.
  2. 지연 스타일은 <'link rel="preload"'> 속성으로 병렬 로딩 후 지연 적용. (ex. 팝업 css)
```html
  <link rel="preload" as="style" href="*.css" onload="this.onload=null;this.rel='stylesheet'">
``` 

  👉 **this.onload=null 할당이유: **
  rel 속성을 변경할 때 일부 브라우저가 다시 onload 실행하는 것을 방어하는 것
  
 [https://t.ly/GiGe](https://t.ly/GiGe)

 [https://is.gd/HuwPS2](https://is.gd/HuwPS2)


![](https://images.velog.io/images/dev_jihee/post/e7bb72be-0c3a-4c9c-bd65-44e141936afe/image.png)


  👉 외부 스타일 파일이 렌더링(FCP)을 차단하지 않음

  
  ## Summary  
  1. 웹 브라우저는 외부 JS, CSS 파일을 로딩 하고 파싱 하는 동안 렌더링 차단 상태를 유지한다.
  2. 사용하지 않는 JS, CSS 제거
  3. 필수 코드는 페이지에 <'style'>...<'/style'>, <'script'>...<'/script'> 작성하기
  4. 필수 아닌 JS는 <'/body'> 종료 직전 위치를 고려. defer, async 속성을 사용.
  5. 필수 아닌 CSS는 병렬 로딩(preload)하고 지연 적용(onload)하기.