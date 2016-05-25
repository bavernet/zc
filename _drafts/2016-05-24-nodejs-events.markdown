---
layout: post
title:  "NodeJS - Event Handling"
date:   2016-05-23 09:31:02 +0900
categories: NodeJS
excerpt: "나머지 연산에서의 나눗셈을 제외한 사칙연산은 분배법칙이 성립한다.<br>
이는 나머지연산은 정수범위에서 가능하고 나눗셈을 제외한
덧셈, 뺄셈, 곱셈만이 정수에 닫혀 있기 때문에 발생하는 성질이다.<br>
여기서는 나눗셈을 곱셈의 역원으로 변형하여 연산할 수 있는 방법을 알아본다."
---

## Modules

module은 파일별로 구분되며, 실제 모듈은 immediate function으로 감싸서 만들어진다.

{% highlight nodejs %}
(function (exports, require, module, __filename, __dirname) { var calc = require("./calc.js");
console.log(calc);

var r = calc.plus(10, 20);
console.log(r);

r = calc.minus(20, 10);
console.log(r);
});
{% endhighlight %}


require()를 이용해서 외부 module의 함수를 사용할 수 있다.
이 때, require() 내부에 가져올 파일의 정확한 경로를 지정하지 않고, 파일이름만 명시할 경우
가장 가까운 node_modules 디렉토리에서 검색하여 로드한다.
파일이름의 확장자를 명시하지 않을 경우 .node 와 .js 파일을 찾아서 로드한다.

파일도 존재하지 않을 경우 디렉토리 이름을 찾고,
디렉토리 내에서 package.json 파일을 먼저 찾아보고 없으면 index.js를 찾아서 로드한다.

{% highlight json %}
{
	"name" : "mymodule4",
	"main" : "mymodule4.js"
}
{% endhighlight %}

require()를 이용해서 외부 module을 로드한 경우 module 내에서 exports 혹은 module.exports에
property로 만들어 놓은 객체들만 사용할 수 있다.

같은 모듈에 대해서 require()를 여러번 호출한다고 하더라도 여러번 로드하지 않고 단 한번만 로드한다.


## 변수선언


## Data Type

실제 변수에 값이 할당될 때, 변수의 타입이 정해지며 그 순간의 변수타입을 알고 싶다면
``typeof`` 라는 키워드로 알아낼 수 있다.

1. Undefined
1. Null
1. Number
  1. NaN: Not a Number
    - isNaN() : 숫자로 변환한 다음에 NaN이 되면 이 함수를 쓴다
    - Number.isNaN() : 원래 값이 NaN이면 이 함수를 쓴다
1. String
1. Boolean
1. Object

데이터 타입을 자바스크립트 나름대로 최대한 의도한 방식의 값으로 형변환을 시도한다.

 - Number로 형변환
  - -, *, /
  - .valueOf() 함수로 변환
 - String로 형변환
  - .toString() 함수로 변환

** toString()과 valueOf()가 모두 존재할 경우 항상 valueOf()가 우선시된다 **

** parseInt()는 중간에 문자가 들어있어도 변환할 수 있는 곳까지는 숫자로 변환시킨다 **
** Number()는 전체가 모두 숫자로 변환할 수 없으면 NaN을 반환한다 **


항상 부동소수점 연산을 수행하기 때문에 피연산자가 모두 정수형이더라도 부동소수점형을 반환할 수 있다
0으로 나누는 것은 에러가 아니고 Infinity 형이 반환되며, 이는 isFinite() 함수로 구분할 수 있다.

obj의 property는 아래 두가지 방법으로 참조가능하다

1. obj.name
1. obj["name"]

모든 기본값은 값에 의한 복사가 일어나고, obj는 참조에 의한 복사가 일어난다.


### Array

var array = new Array();
var array = new Array(5);
var array = new Array(1, 2, 3);

var array = [];
var array = [ , , , , ];
var array = [1, 2, 3];

array.length=5;

.push(), .pop(), .shift(), .unshift()

http://www.w3schools.com/jsref/default.asp


{% highlight python %}
{% endhighlight %}


### Closure

{% highlight python %}
var x = 'global';
function global() {
	var x = 'local';
	return function() {
		console.log(x);
	}
}
var local = global();
local();	// 'local' 출력됨
{% endhighlight %}


### Class

{% highlight python %}
var x = 'global';
function Person(name, age) {
	this.name = name;
	this.age = age;
}

var m = new Person('Bae', 28); // this는 new로 인해 반환된 object를 지칭한다.
{% endhighlight %}


Prototype은 function()을 통해서 생성될 때 자동으로 생성되고, 해당 생성자를 통하는 모든 것들끼리
공유되는 Object로 object의 method이나 static variable을 만들 때 사용된다.

자바스크립트는 스택이 존재하긴 하지만 함수 내부적으로는 실행될 때마다 실행객체를 만들어서
지역변수를 저장하고, 해당 실행객체는 힙에 존재하기 때문에 스택이 없어지더라도 참조하는 링크가
남아있다면 여전히 힙상으로 존재하게 된다.

#### 상속하는 방법

{% highlight python %}
function Super() {
}
//Super.prototype = new Object();
//Super.prototype.constructor = Super; // 단지 sup.constructor를 표현하는 방식만 다르다

Super.prototype.sayHello = function() {
	console.log('hello this is a super class');
}

function Sub() {
}

Sub.prototype = new Super();

var sup = new Super();
sup.sayHello();

var sub = new Sub();
sub.sayHello();

console.log(sub instanceof Sub);
console.log(sub instanceof Super);
console.log(sub instanceof Object);

{% endhighlight %}

#### 동적 바인드

{% highlight python %}
function Person(name, age) {
	this.name = name;
	this.age = age;
}

Person.prototype.sayHello = function() {
	console.log(this.name + ": " + this.age);
}

var lee = new Person('lee', 27);
lee.sayHello();

function Apple(name, age) {
	this.name = name;
	this.age = age;
}

var busa = new Apple('busa', 50);
Person.prototype.sayHello.call(busa);


function f(i, j) {
	console.log('hello');
	console.log(arguments);
}

f(1, 2);
f.call(null, 2, 3);
var arr = [ 3, 4 ];
f.apply(null, arr);
{% endhighlight %}

this는 동적 바인드가 가능하기 때문에 호출하는 시점에 어떻게 쓰일지 결정하게 된다.
함수를 전달하는 API를 작성하게 될 경우 this가 의미하는 것이 무엇인지를 document에 꼭
명시해주어야 한다. 왜냐면 호출하는 시점에 this가 결정되기 때문에 어떻게 사용할지 모르는
문제가 발생한다.


나눗셈은 정수를 넘어서 실수에 닫혀있는 연산이기 때문이다.

그렇다면 나눗셈과 나머지 연산이 결합하면 방법이 없는 것일까?

결론적으로 말하면 아니다.
단지 형태가 우리가 생각하는 결합법칙과는 조금 다를 뿐 풀 수 있는 방법이 존재한다.

$$ (a\;/\;b)\;\%\;M $$

그럼 위의 식을 어떻게 전개할 수 있을까?<br>
일단 나눗셈은 바로 전개가 되지 않는다고 했으니,
전개가 되는 연산인 곱셈으로 바꿔 생각해보자.<br>
그럼 위의 식을 아래와 같이 바꿀 수 있다.

$$ (a * b^{-1})\;\%\;M = ((a\;\%\;M) * (b^{-1}\;\%\;M)) $$

이처럼 곱셈에 대해서 분배법칙을 적용할 수 있고,
$$ (b^{-1}\;\%\;M) $$ 에 대해서만 해결할 수 있다면 문제의 해답을 찾을 수 있다.

$$ b^{-1} $$은 곱셈의 역원으로 볼 수 있기 때문에
앞으로는 나눗셈을 구한다기보다는
나눗셈이 곱셈의 역원이라는 관점에서 문제를 풀어나가도록 한다.


## 나머지 연산에 대한 곱셈의 역원 구하기

나머지 연산에 대한 곱셈의 역원을 구하는 방법은 아래 2가지가 존재한다.

1. **페르마의 소정리 (Fermat's Little Theorem)**<br>
1. **확장 유클리드 호제법 (Extended Euclidean Algorithm)**

여기서는 그 두가지 방법을 이용하여 곱셈의 역원을 구하는 방법과
이에 대한 증명을 하고자 한다.

### 페르마의 소정리 (Fermat's Little Theorem)

페르마의 소정리는 a이 소수 p로 나눠떨어지지 않을 경우 항상 아래식을 만족한다는 것이다.

$$ a^{p-1}\;\%\;p = 1 $$

``단, a와 p는 서로소이어야 한다``

이 수식의 양변에 $$ a^{-1} $$을 곱하면 다음과 같은 결과가 얻어지고,

$$ (1 * a^{-1})\;\%\;p = (a^{p - 1} * a^{-1})\;\%\;p $$<br>
$$ a^{-1}\;\%\;p = a^{p - 2}\;\%\;p $$

이를 이용하여 우리가 원하던 나머지 연산에 대한 나눗셈은 아래와 같은 형태로 전개할 수 있다.

$$ \therefore \; (a / b)\;\%\;M = ((a\;\%\;M) * (b^{-1}\;\%\;M)) = ((a\;\%\;M) * (b^{M - 1}\;\%\;M)) $$

``단, b와 M는 서로소이어야 한다``


#### 페르마의 소정리 증명

페르마의 소정리의 증명은 그렇게 어렵지 않다.

소수 p에 대해 아래와 같은 두 종류의 집합을 생각해보자.

$$ A = \{ 1, 2, 3, 4, ... , p - 2, p - 1 \} $$<br>
$$ B = aA = \{ a, 2a, 3a, 4a, ... , a(p - 2), a(p - 1) \} $$

``단, a와 p는 서로소이어야 한다``

위의 수식에서 확인할 수 있는 것처럼
A 집합은 소수 p 보다 작은 수들로 이루어져 있기 때문에 나머지 연산을 적용하더라도
집합의 원소는 달라지지 않는다.

그럼 A 집합에 p와 서로소인 정수 a를 곱한 집합 B는 어떨까?<br>
$$ a $$와 $$ p $$는 서로소이기 때문에 서로 나눠떨어지지 않는다는
것에 초점을 맞춰 생각해보면 쉽게 알 수 있다.

만약 쉽게 납득이 가지 않는다면 2개의 수직선을 상상해보자.<br>
$$ L1 $$는 $$ a $$ 간격으로 $$ a, 2a, 3a, ..., ap $$ 위치에 눈금이 있는 직선이고,
$$ L2 $$는 $$ p $$ 간격으로 $$ p, 2p, 3p, ..., ap $$ 위치에 눈금이 있는 직선이다.

그 두 직선을 겹쳐보면 $$ a $$와 $$ p $$는 서로소이기 때문에
처음과 마지막 눈금을 빼면 겹치는 눈금이 없고,
각 직선에 표시된 눈금의 개수는 상대방 직선의 간격만큼인 것을 알 수 있다.<br>
두 직선의 마지막 눈금은 모두 $$ ap $$로 $$ a $$가 $$ p $$개
혹은 $$ p $$가 $$ a $$개 있다고 해석할 수 있기 때문이다.<br>
이제 $$ L1 $$을 $$ p $$의 간격으로 접어보면 $$ 1, 2, 3, ... , p - 2, p - 1 $$ 에
$$ L1 $$직선의 눈금이 위치한 것을 알 수 있다.

따라서 이를 수식으로 정리하면 아래와 같다.

&nbsp;&nbsp;&nbsp;&nbsp;$$ B\;\%\;p = \{ a\;\%\;p, 2a\;\%\;p, 3a\;\%\;p, 4a\;\%\;p, ... , a(p - 2)\;\%\;p, a(p - 1)\;\%\;p \} $$<br>
&nbsp;&nbsp;&nbsp;&nbsp;$$ B\;\%\;p = \{ 1, 2, 3, 4, ... , p - 2, p - 1 \} = A $$<br>
$$ \therefore A\;\%\;p = B\;\%\;p $$

이제 위의 집합에 대한 증명을 이용해서 페르마의 소정리를 증명해보자

위의 두 집합 $$ A $$와 $$ B $$에 $$ p $$에 대한 나머지 연산을 적용하면 집합원소가 같아지기 때문에,
내부 집합원소를 모두 곱한 경우도 나머지 연산을 적용하면 같은 값을 얻을 수 있다는 것을 쉽게 알 수 있다.<br>
따라서, 이를 이용하면 아래 수식이 성립함을 알 수 있다.

$$ 1 * 2 * 3 * ... * (p - 2) * (p - 2) = (p - 1)! $$<br>
$$ (p - 1)!\;\%\;p = a^{p - 1}(p - 1)!\;\%\;p $$<br>
$$ 1 = a^{p - 1}\;\%\;p $$


### 확장 유클리드 호제법 (Extended Euclidean Algorithm)

많은 사람들이 **Euclidean Algorithm**은 익숙할 것이다.
적어도 전산학을 전공한 사람이라면, 최대공약수(GCD)를 구하는 프로그램을 한번쯤은 만들어봤을 것이고,
이 때 빼놓을 수 없는 것이 **Euclidean Algorithm**이다.

이름에서 알 수 있듯이 **<font color="red">Extended Euclidean Algorithm</font>**은
$$ gcd(a, b) $$를 구하는 **Euclidean Algorithm**의 확장된 개념으로,
모든 정수에 대해서 아래 식을 만족하는 수가 존재하고,
이 때 $$ x $$와 $$ y $$를 구할 수 있다는 것이 알고리즘의 핵심이다.

$$ gcd(a, b) = ax + by $$

Euclidean Algorithm에서 Extended Euclidean Algorithm을 증명해가는 과정은
첨부파일을 확인하면 쉽게 알 수 있기 때문에 여기서는
Extended Euclidean Algorithm을 이용해서 나머지 연산에 대한 곱셈의 역원을 구하는 것에 집중한다.
Extended Euclidean Algorithm에 대한 증명이 궁금하다면
[Extended Euclidean Algorithm Prove]({{ site.url }}/attach/xeuclid.pdf)<sup>[1]</sup> 를
참조하면 쉽게 이해할 수 있다.

그럼 이제 증명을 시작해보자.<br>
우리가 알고 싶은 것은 나머지 연산에서 곱셈에 대한 역원이다.<br>
즉, 곱셈 연산을 통해 항등원인 1을 만드는 방법을 알고 싶은 것이고,
a에 대해 아래식을 만족하는 x를 찾으려는 것이다.

$$ ax\;\%\;M = 1 $$

그럼 이제 Extended Euclidean Algrithm을 이용하여 위의 식을 유도해보자.

먼저 Extended Euclidean Algorithm의 기본식을 조금만 변형하면 아래와 같은 식을 얻을 수 있다.

$$ ax + My = gcd(a, M) $$

이 식의 양변에 M에 대한 나머지 연산을 적용해보자.

$$ (ax + My)\;\%\;M = (ax\;\%\;M) + (My\;\%\;M) = (ax\;\%\;M) + 0 $$

$$ ax\;\%\;M = gcd(a, M)\;\%\;M $$

``단, a와 M은 서로소이어야 한다``

이 때, a과 M가 서로소의 관계라면, 즉 $$ gcd(a, M) = 1 $$ 이라면,
우리가 유도하고자 했던 식이 완성된다.

따라서 나머지 연산에 대한 곱셈의 역원은 $$ gcd(a, M) = 1 $$ 일 때,
Extende Euclidean Algorithm의 해법으로 구할 수 있고,
그 해법은 아래와 같다.

{% highlight python %}
def xgcd(a, b):
	if b == 0:
		return [1, 0, a]
	x, y, d = xgcd(b, a % b)
	return [y, x - (a // b) * y, d]

def inv(a, M):
	x, y, d = xgcd(a, M)
	return x
{% endhighlight %}


## 참고자료

[1] <http://pages.pacificcoast.net/~cazelais/222/xeuclid.pdf>


