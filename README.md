CSS 수퍼파워 Sass로 디자인하라
=========================
이 글은 [CSS 수퍼파워 Sass로 디자인하라](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&barcode=9788997924233) 책을 토대로 공부한 내용을 정리한 글입니다.

## `시작하기`
## 1부. CSS 수퍼파워 - Sass
- Syntactically Awesome Style Sheet
- Sass를 통해 스타일시트 작업을 프로그래밍하는 것처럼 체계적이고, 구조적으로 개발할 수 있다.

### 1장. Sass 사용을 위한 준비 단계
- Sass 파일은 단독으로 웹 사이트에서 사용할 수 없다.
- 컴파일을 통해 css로 변환되어야 한다.

#### 1.1 PC에 Sass 설치하기
`npm install -g sass`
- npm을 통해 Sass 를 설치한다.
- 기타 설치 방법은 [Sass 공식 홈페이지](https://sass-lang.com/install)에서 확인한다.

`npm install node-sass`
- sass 파일의 컴파일을 위해 node-sass를 설치한다.

`node-sass foo.scss > bar.css`
- 특정 파일을 특정 파일 이름을 컴파일하는 예시이다.

`node-sass src/sass --output dist/css`
- 폴더 내의 모든 파일을 컴파일하는 예시이다.

``node-sass --watch src/sass --output dist/css``
- 실시간으로 Sass 파일을 감시하여 파일이 변경될 때마다 컴파일하여 css 파일을 업데이트하는 watch 모드 예시이다.

- Sass 파일을 컴파일하여 css 파일을 생성할 때 아래 4가지 스타일 중 하나를 선택할 수 있다.

```js
// nested : Sass 형식과 유사하게 nest된 css 파일이 생성된다.(기본값)
node-sass --output-style nested src/sass --output dist/css

// expanded : 표준적인 스타일의 css 파일이 생성된다.
node-sass --output-style expanded src/sass --output dist/css

// compact : 여러 룰셋을 한줄로 나타내는 스타일의 css 파일이 생성된다.
node-sass --output-style compact src/sass --output dist/css

// compressed : 가능한 빈 공간이 없는 압축된 스타일의 css 파일이 생성된다.
node-sass --output-style compressed src/sass --output dist/css
```

## `1일차`
#### 1.2 Sass의 기초
- Sass에는 .sass와 .scss 가 있다. 두 파일의 차이점은 중괄호({})dhk 세미콜론(;)의 유무라고 할 수 있다.
```scss
// .scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;
body {
    font: 100% $font-stack;
    color: $primary-color;
}
```
```scss
// .sass
$font-stack: Helvetica, sans-serif
$primary-color: #333
body
    font: 100% $font-stack
    color: $primary-color
```
- 두 방식을 혼용할 수 없으며, 편한 방식을 선택하여 작성하면 된다.
- Sass는 변수를 사용할 수 있고, 중첩(Nesting) 방식으로 코딩할 수 있다.
- 파편화(Partials)해서 다른 파일에 불러(import)올 수 있다.
- Mixin 방식으로 css3 속성 중 브라우저별로 속성값을 지정하는 경우, 간단히 처리할 수 있다.
- 확장(extend)과 상속(inheritance)를 이용하여 동일한 속성값을 반복적으로 사용하는 것을 편리하게 처리할 수 있다.

*변수*
- Sass에서 변수를 정의할 때 $ 기호를 사용한다.
```scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;
body {
    font: 100% $font-stack;
    color: $primary-color;
}
```

*중첩(Nesting)*
- 기존 css를 Sass의 중첩을 이용하여 변경하면 아래와 같습니다.

```css
/* css */
nav ul {
    list-style: none;
}
nav ul li {
    display: inline-block;
}
nav ul li a {
    display: block;
    padding: 5px 3px;
    margin: 10px 20px;
    text-decoration: none;
    color: #999;
}
nav ul li a:hover {
    border-bottom: 1px dotted #999;
}
```
```scss
// sass
$default-color: #999;
$navhover-color: #ffb069;

nav {
    ul {
        list-style: none;
        li{
            display: inline-block;
            a {
                display: blcok;
                padding: 5px 3px;
                margin: 10px 20px;
                text-decoration: none;
                color: $default-color;

                &:hover {
                    border-bottom: 1px dotted $navhover-color;
                    color: $navhover-color;
                }
            }
        }
    }
}
```
- 중첩의 장점은 코드의 길이를 많이 줄여줄 수 있다는 것과 HTML의 박스 모델에서 각 박스에 있는 개별 태그에 정확한 값을 지정해 줄 수 있다는 것이다.

*부분화(Partials) / 불러오기(Import)*
- Sass 작업 시 코드의 길이가 길어지고 복잡하게 되어 분리할 필요가 있을 때 코드를 분리하는 것을 '부분화(Partials)'라고 한다.
- 부분적으로 쪼갠 Sass 파일을 하나의 파일에 '불러오기(Import)'한 후 컴파일을 거치면 완벽한 하나의 css 파일로 변환된다.
- 부분화를 할 때 가장 중요한 것이 파일의 이름이다. 부분화를 하는 파일 이름은 앞 부분에 반드시 '_(언더바)'를 추가해야 한다.

```scss
@import 'color';
```
- 파일 확장자를 붙이지 않고, 부분화 파일명 앞에 있는 _(언더바)도 붙이지 않는다.

*Mixins*
- mixin은 브라우저별 접두사를 처리하거나 반복적인 속성을 손쉽게 처리할 수 있게 해주는 역할을 한다.

```scss
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box {
    @include border-radius(10px);
}
```
- Sass의 가장 큰 장점은 반복되는 작업을 최대한 줄여주고, 생산성을 높이며, 작업의 효율성을 극대화하는 데 있다.

*확장(Extend) / 상속(Inheritance)*
- Sass 에서 확장은 @extend 라는 코드를 이용한다.

```scss
.message {
    border: 1px solid #ccc;
    padding: 10px;
    color: #333;
}
.success {
    @extend .message;
    border-color: green;
}
.error {
    @extend .message;
    border-color: red;
}
.warning {
    @extend .message;
    border-color: yellow;
}
```
- .message 를 확장하여 메시지 종류에 따라 별도의 border-color를 지정했다.

*연산자(Operators)*
- Sass는 프로그래밍적 요소가 있기 때문에 간단한 사칙연산이 가능하다.

```scss
.container {
    width: 100%;
}
article[role="main"] {
    float: left;
    width: 600px / 960px * 100%;
}
article[role="complimentary"] {
    float: right;
    width: 300px / 960px * 100%;
}
```
- 웹 사이트의 width가 유동적으로 변경되는 경우, 콘텐츠가 드어가는 부분과 사이드 바의 크기 또한 유동적으로 변경해 줄 필요가 있다.
- 이 경우 연산 기능을 이용하면, 해당 콘텐츠의 크기를 쉽게 처리할 수 있다.

### 2장. Sass 레퍼런스
#### 2.1 개요
- 