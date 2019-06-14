---
layout: post
title:  "[CSS] display 속성: inline과 block의 차이"
date:   2019-03-26
categories: CSS
---

CSS의 주요속성 중 하나인 display 속성과, 속성값 중 inline과 block이 어떤 차이가 있는지 정리한 글입니다.

## display 속성
요소를 화면에 보이거나 숨길 때 사용한다.
* visibility와의 차이 : visibility는 요소를 보일지 말지 결정, display는 보여지는 방법 결정

### inline
기본 값, 앞뒤로 줄바꿈되지 않는다.

### block
요소 앞 뒤로 줄바꿈된다. (div는 block 요소)

### none
박스가 생성되지 않으므로, 공간을 차지하지 않는다.

### inline-block 
요소는 inline인데 내부는 block처럼 표시되고, 박스 모양이 inline처럼 옆으로 늘어선다.

***

## inline 요소와 block 요소
HTML 태그는 크게 inline요소와 block요소로 나뉘며 각 요소에 적용되는 CSS가 다르다.

### inline 요소
항상 block 요소 안에 포함되며, 인라인 요소 안에 다른 인라인 요소 포함 가능하다.

* **기본** : 컨텐츠가 끝나는 지점까지를 넓이로 가짐
* width, height로 임의로 변형을 줄 수 없음
* line-height로 줄간격을, text-align으로 텍스트 정렬을 설정 가능
* 인라인 요소 다음에는 줄바꿈 없이 바로 이어서 보임

#### inline 요소 태그의 종류

`a`, `abbr`, `acronym`, `b`, `bdo`, `big`, `br`, `button`, `cite`, `code`, `dfn`, `em`, `i`, `img`, `input`, `kbd`, `label`, `map`, `object`, `q`, `samp`, `small`, `script`, `select`, `span`, `strong`, `sub`, `sup`, `textarea`, `tt`, `var`

### block 요소
모든 inline요소를 포함할 수 있고, 다른 block 요소도 포함할 수 있다.

* **기본** : 가로폭 전체의 넓이를 가지는 직사각형 형태
* 블록요소 다음에는 줄바꿈이 이루어짐
* display: inline 설정으로 블록 요소를 인라인 요소로 변경할 수 있음
* block 요소에 text-align을 주면 요소 자체가 아닌, 내부의 모든 인라인 요소에 적용되는 것

#### block 요소 태그의 종류
`address`, `article`, `aside`, `audio`, `blockquote`, `canvas`, `dd`, `div`, `dl`, `fieldset`, `figcaption`, `figure`, `footer`, `form`, `h1`, `h2`, `h3`, `h4`, `h5`, `h6`, `header`, `hgroup`, `hr`, `noscript`, `ol`, `output`, `p`, `pre`, `section`, `table`, `ul`, `video`
