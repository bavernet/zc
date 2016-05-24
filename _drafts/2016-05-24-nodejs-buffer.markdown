---
layout: post
title:  "Buffer vs Array in NodeJS"
date:   2016-05-24 16:03:24 +0900
categories: NodeJS, Buffer
excerpt: "NodeJS의 Buffer와 Array의 차이를 알아본다."
---

실제 여러 8-bit의 공간으로 구성된 C에서 생각하는 char array와 동일한 형태이다.
Javascript의 Array와의 차이점은
Javascript의 Array는 각 공간이 동일한 크기가 아니고 여러 객체를 담을 수 있는 Wrapper Container로
간주할 수 있다.
그래서 여러개의 변수를 선언하는 것과 동일하지만, 브라우저의 최적화에 의해 수행 속도는
Array를 통해서 접근하는 것이 여러개의 변수를 독립적으로 선언하는 것보다는 빠르다.

|                | Javascript         | Array                  |
|----------------|--------------------|------------------------|
| Container      | Yes                | Yes                    |
| Memory Mapping | Yes                | No                     |
| Usage          | Binary             | Others                 |
| Element        | 8-bit memory space | reference of an object |

{% highlight nodejs %}
var buff = new Buffer(1024);
var array = new Array(1024);

console.log(buff[12]);
console.log(array[12]);

buff[0] = 1;
buff[1] = 2;
buff[2] = 255;
buff[3] = 256;
for (var i = 0; i < buff.length; ++i) {
	console.log(buff[i]);
}

console.log("buff[2]", buff[2]);
console.log("buff[3]", buff[3]);

var buff2 = new Buffer("ABCDabcd한글", "utf-8");
console.log(buff2[0]);
console.log(buff2[1]);
console.log(buff2[2]);
console.log(buff2);
console.log(buff2.toString());

var buff3 = buff2.slice(4, 8);
buff2[4] = 67;
console.log(buff3.toString());

var buff4 = new Buffer(buff2.length);
buff2.copy(buff4, 0, 0, buff2.length);
console.log(buff4.toString());
console.log(buff4.constructor.name);
console.log(buff4.__proto__.__proto__.constructor.name);
{% endhighlight %}


