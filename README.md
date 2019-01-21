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

## `2일차`
### 2장. Sass 레퍼런스
#### 2.1 개요
- [Sass 레퍼런스 URL](http://sass-lang.com/documentation/file.SASS_REFERENCE.html)

#### 2.2 CSS 확장(CSS Extensions)
- css 확장에는 위에서 알아봤던 중첩뿐만 아니라 태그 선택자와 연계된 클래스 또는 아이디 선택자끼리 쉽게 묶어주는 기능도 있다.
- & 기호를 이용해서 처리하며, 수도선택자를 지정할 때도 사용된다.
- 또한 css 확장에는 css 속성 중 동일한 단어가 들어가 있는 속성들, 예를 들어, font-family, font-weight 와 같은 속성들도 중첩을 이용하여 쉽게 처리할 수 있다.

```scss
@import url(http://fonts/googleapis.com/earlyaccess/nunumgothic.css);
$bgcolor: #007bfff;     // 색상을 변수로 지정하여 직관적으로 작업을 처리할 수 있다.
$white: #fff;
$black: #000;

article {
    $.section1 {        // 중첩을 이용하면 이렇게 태그 선택자와 결합된 클래스 또는 아이디 선택자가 있을 경우, & 기호로 간단히 처리할 수 있다.
        width: 800px;
        margin: 10px auto;
        font: {         // 선택자의 이름과 더불어 별도의 속성이 들어가는 부분을 중첩을 통해 간편하게 처리할 수 있다.
            size: 14px;
            family: 'Nanum Gothic', sans-serif;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 50px;
            th,td {
                padding: 12px 8px;
                text-align: center;
                border: 1px solid rgba($black, 0.4);    // 색상이 변수로 지정되어 있다면, 변수값을 넣고 투명도를 지정하게 되면,
            }                                           // 최종 css 파일에는 rgba(0, 0, 0, 0.4); 로 변환된다.
            th {
                background-color: rgba($bgcolor, 0.8);
                color: $white;
                border: {       // 선택자의 이름과 더불어 별도의 속성이 들어가는 부분을 중첩을 통해 간편하게 처리할 수 있다.
                    top: 2px solid $black;
                    bottom: 2px solid $black;
                }
            }
            td {
                $:nth-child(2) {        // 수도선택자(psuedo selector)를 & 기호로 간단히 처리할 수 있다.
                    text-align: left;
                    padding-left: 15px;
                }
            }
            tr {
                $:hover {               // 수도선택자(psuedo selector)를 & 기호로 간단히 처리할 수 있다.
                    background-color: rgba($bgcolor, 0.6);
                    color: $white;
                    cursor: pointer;
                }
            }
        }
    }
}
```
- 참고로 & 를 뒷 부분에 붙일 수도 있다.

```scss
.select1 {
    padding: 30px;
    .another & {        // '.another .select1 {' 과 동일하다.
        margin: 20px;
    }
}
```
- 즉, Sass에서 & 는 부모 선택자를 의미하며, 부모 선택자와 해당 선택자를 묶는 역할 또는 부모 선택자를 다른 선택자의 하위 선택자로 처리할 수 있다는 것을 의미한다.

#### 2.3 주석 처리
```scss
/* 주석 내용 */
이 방식은 Sass 컴파일 결과물인 css 파일에도 그대로 반영된다.
css 파일에 저작권 또는 css 파일 버전 등을 표시할 때 사용한다.

// 주석 내용
이 방식은 css 파일에 전혀 반영되지 않는다.
Sass의 속성을 지정하여 유지보수 할 때 편리하다.
```
- **단, Sass 에서 한글로 주석을 달면 컴파일 시 에러가 발생하니 주의해야 한다.**

#### 2.4 SassScript
`sass -i`
- 인터렉티브 쉘(Interactive Shell) 상태에서 SassScript를 실행하겠다는 의미다.
- 사칙연산이 가능하며, 색상 코드를 더할 수도 있다.

#### 2.5 변수: $ 기호를 사용
- Sass 에서는 전역(global) 변수와 지역(local) 변수가 존재한다.
- 전역변수는 선택자 외부에 별도로 선언을 하거나, 지역변수 부분에 !global이라고 지정하면 전역변수가 된다.

```scss
$main-width: 100%;                  // 전역변수
#main {
    $width: 10px !global;           // 전역변수
    border: $width solid red;       // 전역변수 $width 사용
    $main_color: #fff;
    background-color: $main-color;
    width: $main-width;
}
#sidebar {
    width: $width*50;               // 전역변수 $width 사용
}
#content {
    $width: 500px;
    width: $width;                  // 지역변수 $width 사용
}
#sidebar2 {
    width: ($main_width/10);        // Sass 에서는 하이픈(-)과 언더바(_)를 동일한 기호로 인식한다.
}
```
- 많은 Sass 개발자들은 보통 _variables.scss 라는 별도의 파일로 만들어서 전역변수를 분리한다.

#### 2.6 데이터 타입
- SassScript는 다음과 같은 7가지의 데이터 타입을 지원한다.

```
1. 숫자 - numbers (예: 1.2, 13, 10px)
2. 문자열(따옴표 포함한) - strings of text, with and without quotes (예: "foo", 'bar', baz)
3. 색상값 - colors (예: blue, #04a3f9, rgba(255,0,0,0.5))
4. 불린함수 - booleans (예: true, false)
5. 널값 - nulls (예: null)
6. 수치값(폰트명) - lists of values, separated by spaces or commas (예: 1.5em 1em 0 2em, Helvetica, Arial, sans-serif)
7. 맵스 - maps from one values to another (예: value1, key2,: value2)
```
- 모든 종류의 css 속성 값들을 지원하고, !important 선언도 지원한다.

*문자열*
- SassScript 에서는 일반 텍스트, 따옴표, 쌍따옴표 모두 동일하게 취급한다.
- font-family를 지정하는 경우 폰트 이름이 2개의 단어로 구성된 경우, 쌍따옴표 또는 따옴표를 사용한다.
- URL을 표시하는 경우, 쌍따옴표 또는 따옴표를 사용한다.
- 인용(interpolation)을 적용하는 경우에는 선택자 부분에 반드시 (쌍)따옴표를 사용해야 한다.

*리스트*
- 리스트는 배열을 의미하며, 변수에도 배열을 사용할 수 있다.
- 단, 배열 중간에 null 값을 넣을 수 없다. (버본이라는 Sass 프레임워크에서는 가능하지만, 이는 극히 예외적인 상황이다.)

*맵스(Maps)*
- 맵스는 다음과 같이 키와 그 키 값에 대한 배열을 지정하는 것이다.

```scss
$map: (key1: value1, key2: value2, key3: value3);
```

*색상*
- css 에서 사용되는 어떠한 색상 코드도 Sass에서 사용이 가능하다.
- 특히 색상을 16진수로 표현한 후 Sass 본문에서 rgba 코드 값을 사용하면 매우 편리하다.

#### 2.7 연산(Operations)
*숫자 연산(Number Operations)*
- 연산에서는 사칙연산 기호를 모두 사용할 수 있다.
- 단, font 속성에서 "/" 기호는 아래와 같은 의미로도 사용된다.

```scss
font: 10px/8px;     // 여기서 "/" 기호는 font-size와 line-height 값을 구분하는 의미로 사용된다.

font-size: 10px;
line-height: 8px;
```

*색상 연산(Color Operations)
```scss
a {
    color: rgba(255, 0, 0, 0.75) + rgba(0, 255, 0, 0.75);
}

// 결과는
a {
    color: rgba(255, 255, 0, 0.75);
}
```
- rgba 또는 hsla로 구성된 색상의 경우, 연산을 위해서는 반드시 동일한 알파값을 넣어줘야 한다.
- Sass 에서 알파값은 opacity와 transparentize 속성으로 조절 가능하다.

```scss
$red50: rgba(255, 0, 0, 0.5);

div {
    color: opacity($red50, 0.3);
    background-color: transparentize($red50, 0.25);
}

// 결과는
div {
    color: rgba(255, 0, 0, 0.8);                // 알파값이 0.3 증가(opacity 속성 효과)
    background-color: rgba(255, 0, 0, 0.25);    // 알파값이 0.25 감소(transparentize 속성 효과)
}
```

*문자열 연산(String Operations)*
- 문자열 연산은 두 개의 문자열을 결합하는 것을 의미한다.

```scss
p:before {
    content: "Foo" + Bar;
    font-family: sans- + "serif";
}

// 결과는
p:before {
    content: "Foo Bar";
    font-family: sans-serif;
}
```

*삽입(Interpolation)*
- #{} 구문을 사용하여 선택자와 속성 이름에 변수를 제공한다.

```scss
$name: foo;
$attr: border;

p.#{$name} {
    #{$attr}-color: blue;
}

// 결과는
p.foo {
    border-color: blue;
}
```

*변수 기본값 설정(Variable Defaults: !default)*
- 변수에 default 값을 지정하려는 경우, 해당 변수 옆에 !default 라고 적어주면 된다.
- !default 의 정확한 의미는, 해당 변수의 다른 값이 이미 있다면 무시하고, 값이 없다면 해당 변수 값을 할당하라는 의미다.

```scss
$content: "First content";
$content: "Second content?" !default;
$new_content: "First time refernce" !default;

#main {
    content: $content;
    new-content: $new_content;
}

$content1: "First content";
$content1: "Second content?";
$new_content2: "First time reference" !default;
$new_content2: "Second time reference";

#main {
    content: $content1;
    new-content2: $new_content2;
}

// 결과는
#main {
    content: "First content";               // default 값 외에 다른 값이 이미 존재하므로 다른 값이 default 값은 무시된다.
    new-content: "First time refernce";     // default 값 외에 다른 값이 없으므로 default 값이 사용된다.
}

#main {
    content: "Second content?";             // default 값을 선언하지 않았으므로 마지막에 선언한 값이 사용된다.
    new-content: "Second time reference";   // default 값 외에 다른 값이 이미 존재하므로 다른 값이 default 값은 무시된다.
}
```
- 일반적으로 !default 는 사용할 일이 거의 없으나, 부트스트랩과 같은 라이브러리 Sass를 작성하는 경우라면 거의 필수적으로 사용하게 된다.
- 부트스트랩은 아주 많은 변수를 가지고 있으며, _variable.scss에 정의한 모든 변수는 !default 플래그를 가지고 있다.
- 이렇게 변수를 모두 기본값으로 작성해 놓으면, 이 라이브러리를 가져와 사용하는 사람은 원본을 망가트리지 않으면서 쉽게 **변수 값을 사용자화** 할 수 있다.

#### 2.8 @ 규칙과 지시어(@-Rules and Directives)
- Sass 에서는 css3에서 사용되는 모든 @ 규칙을 사용할 수 있다.

*@import(불러오기)*

`@import "color";`
- Sass 파일에서는 확장자를 명시하지 않아도 된다.
- import되는 파일의 경우, 파일명 제일 앞에 '_'(언더바)를 적용해야 별도의 css 파일로 컴파일되지 않는다.
- @import 는 파일 전체를 불러오는 방법도 있지만, 중첩을 이용해서 해당 선택자 부분에만 특정 속성을 불러오는 방법도 있다.

```scss
// import 될 파일의 속성
.box1 {
    margin: 0;
    padding: 0;
}

// import 하는 파일
.boxmodel {
    @import "box1";
    font-size: 12px;
}

// 결과는
.boxmedel {
    font-size: 12px;
}
.boxmodel .box1 {
    margin: 0;
    padding: 0;
}
```
- 위와 같이 사용하는 경우, 중첩 구문에만 사용 가능하며, @mixin이나 if 구문에서는 사용할 수 없다.
- 하지만 @import는 외부 파일을 불러오는 용도로 가장 많이 사용한다.

## `3일차`
*@media*
- @media는 반응형(Responsive) 웹사이트 제작에 많이 사용된다.

```scss
.sidebar {
    width: 300px;
    @media screen and (orientation: landscape) {
        width: 500px;
    }
}

// 결과는
.sidebar {
    width: 300px;
}
@media screen and (orientation: landscape) {
    .sidebar {
        width: 500px;
    }
}
```
- 변수와 삽입을 이용하면 아래와 같은 처리도 가능하다.

```scss
$media: screen;
$feature: landscape;
$value: 500px;
.sidebar {
    width: $value;
    @media #{$media} {
        @media (orientation: $feature) {
            width: $value + 200;
        }
    }
}

// 결과는
.sidebar {
    width: 500px;
}
@media screen and (orientation: landscape) {
    .sidebar {
        width: 700px;
    }
}
```
- Sass의 변수는 구현할 때 조금만 신경쓰면, 매우 다양하게 사용 가능하다.

*@extend(확장)*
- @extend는 기존에 설정되어 있는 속성을 재사용하면서 다른 선택자의 기능을 확장해준다.

```scss
body {
    margin: 0;
    padding: 0;
}
.box1 {
    height: 300px;
    text-align: center;
    padding-top: 50px;
    color: #fff;
    background: {
        image: url("pic1.jpg");
        repeat: no-repeat;
        size: cover;
        position: center;
    };
    h1 {
        font-size: 65px;
        text-transform: capitalize;
    };
    p {
        width: 800px;
        margin: 20px auto;
    };
}
.box2 {
    @extend .box1;
    padding-top: 70px;
    h1 {
        font-size: 45px;
    };
    background-image: url("pic2.jpg");
    p {
        width: 500px;
    };
}
.box3 {
    @extend .box2;
    h1 {
        font-size: 80px;
    };
    background-image: url("pic3.jpg");
    text-shadow: 2px 2px 5px #000;
}
```
- 이렇게 @extend를 이용하게 되면, 중복되는 속성을 간단하게 확장하여 추가적인 속성을 첨가해서 빠르게 웹사이트를 개발할 수 있다.
- @extend로 불러온 선택자 내부에 아무런 속성이 없을 경우 에러가 발생한다. 이런 경우 !optional 플래그를 달아주면 에러를 예방할 수 있다.

```scss
.select1 {
    @extend .select2 !optional;
}
```
- @extend를 사용할 때 주의할 사항은 @extend는 @media 내부에서는 사용할 수 없다는 점이다.

*@at-root*
- 중첩을 사용하다가 특정 선택자를 하위 선택자가 아닌 독립적 선택자로 끌어 올려야 하는 경우, @at-root를 사용한다.

```scss
.select1 {
    padding-top: 10px;
    .select2 {
        margin: 10px;
    }
}
.sel1 {
    padding-top: 10px;
    @at-root .sel2 {
        margin: 10px;
    }
}

// 결과는
.select1 {
    padding-top: 10px;
}
.select1 .select2 {
    margin: 10px;
}
.sel1 {
    padding-top: 10px;
}
.sel2 {
    margin: 10px;
}
```
- 보시다시피 @at-root를 적용한 선택자는 컴파일 후 부모 선택자의 하위 선택자로 적용되지 않는다.
- 또한 @at-root 적용 시 with/without 규칙을 사용하면 더 유용하게 사용할 수 있다.

```scss
@media print {
    .page {
        width: 800px;
        @at-root (without: media) {
            color: red;
        }
    }
}

// 결과는
@media print {
    .page {
        width: 800px;
    }
}
.page {
    color: red;
}
```
- 위와 같이 반응형 웹사이트 개발 시 편리하게 사용할 수 있다.

#### 2.9 제어문 및 표현식(Control Directives & Expressions)
- SassScript 에서는 프로그래밍 언어와 같이 제어문을 이용할 수 있다.

*if(...)*
```scss
.firstClass {
    $color: if(true, blue, red);    // true 이므로 blue 이다.
    color: $color;
}
.secondClass {
    $color: if(false, blue, red);   // false 이므로 red 이다.
    color: $color;
}

// 결과는
.firstClass {
    color: blue;
}
.secondClass {
    color: red;
}
```
*@if*
- if(...)가 참과 거짓에 대한 명제에 사용한다면, @if는 참에 대한 값에만 결과값을 준다.

```scss
$weight: bold;
.txt1 {
    @if $weight == bold {font-weight: bold;}
    @else if $weight == light {font-weight: 100;}
    @else if $weight == heavy {font-weight: 900;}
    @else {font-weight: normal;}
}

// 결과는
.txt1 {
    font-weight: bold;
}
```
- 위와 같이 @if는 @else if와 결합되어 보다 정교한 조건문을 만들어 낼 수 있다.
- @if는 나중에 배울 mixin과 결합하면 더욱 편리하고 유용하게 사용할 수 있다.

*@for*
- @for를 이용하면 반복적인 구문을 쉽게 처리할 수 있다.

`@for $var from <start> through <end>`
- <start>와 <end>에는 숫자가 들어가며, $var에는 어떤 변수명도 가능하다.

```scss
@for $i from 1 through 5 {
    .col-#{$i} {
        width: (100/5*$i)+em;
    }
}

// 결과는
.col-1 {width: 20em;}
.col-2 {width: 40em:}
.col-3 {width: 60em:}
.col-4 {width: 80em:}
.col-5 {width: 100em:}
```

*@each*
- @each는 여러 개의 값을 변수에 각각 대입한다.

`@each $var in 변수값1, 변수값2, 변수값3,...`

```scss
@each $usr in bob, john, bill {
    .#{$usr}-icon {
        background-image: url('/img/#{$usr}.png');
    }
}

// 결과는
.bob-icon {
    background-image: url("/img/bob.png");
}
.john-icon {
    background-image: url("/img/john.png");
}
.bill-icon {
    background-image: url("/img/bill.png");
}
```
- 위 코드를 map 속성을 이용해 변경하면

```scss
$ppl: (usr1:bob, usr2:john, usr3:bill);
@each $key, $usr in $ppl {
    .#{$usr}-icon {
        background-image: url('/img/#{$usr}.png');
    }
}
```
- @each는 한 개 이상의 변수값을 지정해 줄 수도 있다.

```scss
@each $usr, $color, $value in (usr1, black, 1), (usr2, red, 2), (usr3, blue, 3) {
    .#{$usr}-icon {
        background-image: url('/img/#{$usr}.png');
        border: $value+px solid $color;
    }
}

// 결과는
.usr1-icon {
    background-image: url("/img/usr1.png");
    border: 1px solid black;
}
.usr2-icon {
    background-image: url("/img/usr2.png");
    border: 2px solid red;
}
.usr3-icon {
    background-image: url("/img/usr3.png");
    border: 3px solid blue;
}
```
- 위 코드 또한 map을 이용하여 처리가 가능하다.

```scss
$usr1: usr1, black, 1;
$usr2: usr2, red, 2;
$usr3: usr3, blue, 3;
@each $usr, $color, $value in $usr1, $usr2, $usr3 {
    .#{$usr}-icon {
        background-image: url('/img/#{$usr}.png');
        border: $value+px solid $color;
    }
}
```

*@while*
- @while은 어떠한 조건을 충족할 때까지 값을 반복한다.

```scss
$i: 1;
@while $i < 5 {
    .col-sm-#{$i} {
        width: 50*$i +px
    }
    $i: $i + 1;
}

// 결과는
.col-sm-1 {
    width: 50px;
}
.col-sm-2 {
    width: 100px;
}
.col-sm-3 {
    width: 150px;
}
.col-sm-4 {
    width: 200px;
}
```
- @while문은 값의 증감에 따라 다양한 값을 간단한 코드를 통해 많은 경우의 수를 만들 수 있어 편리하게 사용 가능하다.

#### 2.10 Mixin(매우 중요)
- mixin은 Sass에서 가장 쓰임새가 많으며, css에서 반복적인 작업을 획기적으로 줄여줄 수 있다.
- @mixin을 이용하여 정의하고, @include를 이용하여 호출한다.

```scss
@mixin boldtext {
    font: {
        family: 'Malgun Gothic', sans-serif;
        weight: bold;
        size: 42px;
    }
    color: red;
}
.boxsample {
    @include boldtext;
}

// 결과는
.boxsample {
    font-family: 'Malgun Gothic', sans-serif;
    font-weight: bold;
    font-size: 42px;
    color: red;
}
```
- 위는 기본적인 mixin 사용법이며, 여기에 변수를 사용하면 더욱 편리해진다.

```scss
@mixin boldtext($size, $color) {
    font: {
        family: 'Malgun Gothic', sans-serif;
        weight: bold;
        size: $size;
    }
    color: $color;
}
.boxsample3 {
    @include boldtext(24px, #000);
}
.boxsample5 {
    @include boldtext($color: red, $size: 36px);
}

// 결과는
.boxsample3 {
    font-family: 'Malgun Gothic', sans-serif;
    font-weight: bold;
    font-size: 24px;
    color: #000;
}
.boxsample5 {
    font-family: 'Malgun Gothic', sans-serif;
    font-weight: bold;
    font-size: 36px;
    color: red;
}
```
- 기본적으로 @include 시 변수값의 위치를 변경하면 안되지만, .boxsample5와 같이 변수명과 값을 같이 써주면 가능하다.

## `4일차`
- mixin을 가장 편리하게 사용하는 방법 중 하나는 1장에서 언급했던 브라우저 별 속성(vendor prefix)을 정의하는 것이다.

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
- 변수를 이용해서 값을 처리할 경우 변수의 값이 하나가 아니라 여러 개로 구성된 속성은 변수명 뒤에 점 세개(...)를 추가하면 된다.

```scss
@mixin box-shadow($shadow...) {
    -moz-box-shadow: $shadows;
    -webkit-box-shadow: $shadows;
    box-shadow: $shadows;
}

.shadows {
    @include box-shadow(0px 4px 5px #666, 2px 6px 10px #999);
}
```
- (...)은 아래와 같은 방법으로도 사용 가능하다.

```scss
@mixin colors($text, $background, $border) {
    color: $text;
    background-color: $background;
    border-color: $border;
}
$values: #ff0000, #00ff000, #0000ff;
.primary {
    @include colors($values...);
}

$value-map: (text: #00ff00, background: #000ff, border: #ff0000);
.secondary {
    @include colors($value-map...);
}

// 결과는
.primary {
    color: #ff0000;
    background-color: #00ff00;
    border-color: #0000ff;
}
.secondary {
    color: #00ff00;
    background-color: #0000ff;
    border-color: #ff0000;
}
```
- @content를 사용하면 @include 했을 때, 그 선택자 내부의 내용들이 @content 부분에 나타나게 된다.

```scss
@mixin boldtext($size, $color) {
    font: {
        family: 'Malgun Gothic', sans-serif;
        weight: bold;
        size: $size;
    }
    color: $color;
    @content;
}

.boxsample {
    @include boldtext(12px, red);
    border: 1px solid blue;
    padding: 200px;
}
```
- mixin 은 조건문과 같이 활용하면 더욱 편리하다.

```scss
@mixin test($condition) {
    $color: if($condition, blue, red);
    color: $color;
}
.firstClass {
    @include test(true);
}
.secondClass {
    @include test(false);
}

// 결과는
.firstClass {
    color: blue;
}
.secondClass {
    color: red;
}
```

### 3장. SASS 프레임워크: 컴퍼스(Compass)
- Sass에는 두 개의 프레임워크가 있으며, 그것은 "CSS authoring framework for Sass"라고 불리는 컴퍼스와 "SASS를 위한 SASS"라는 버본이다.

#### 3.1 컴퍼스 설치
- 컴퍼스는 루비로 설치한다.

`gem install compass`
- compass 를 설치한다.

#### 3.2 컴퍼스 기초
- 컴퍼스의 구문은 변수, Mixins, 함수로 구성되어 있으며, mixin을 가장 많이 사용한다.