---
layout: post
title:  "HTTP Protocol 개론"
date:   2016-05-24 16:03:24 +0900
categories: NodeJS
tags: NodeJS, network
excerpt: "NodeJS에서 소켓통신하는 방법을 알아본다."
---

## HTTP Request Method

| Action | HTTP   | SQL    |
|--------|--------|--------|
| R      | GET    | SELECT |
| C      | POST   | INSERT |
| U      | PUT    | UPDATE |
| D      | DELETE | DELETE |

## 하나의 명령 실행 ##

### exec() ###

1. 결과가 String으로 저장되어 넘어오기 때문에 출력결과가 많으면 메모를 많이 사용하는 단점이 있다.

### execFile() ###

1. 실제로 shell을 실행하는 것이 아니기 때문에 windows에서 batch file을 실행할 경우는 "cmd.exe" command를 실행시키는 방식으로 실행하여야 한다.



## 병렬 프로그래밍 ##

### spawn() ###

### fork() ###

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


