---
layout: post
title:  "NodeJS 자식프로세스 생성하기"
date:   2016-05-24 16:03:24 +0900
categories: NodeJS
tags: child_process, NodeJS
excerpt: "NodeJS에서 자식프로세스를 생성하는 방법을 알아본다."
---

| exec() | shell command 실행 | 1회 실행 | 결과는 Buffer
| execFile() | File 실행 | 1회 실행 | 결과는 Buffer
| spawn() | 별도의 실행파일 실행 | 프로세스를 실행 후 통신가능 | Stream 연결하여 결과를 받음 |
| fork() | NodeJS를 실행 i.e. spawn("node " + ...) | 프로세스를 실행 후 통신가능 | Stream 연결하여 결과를 받음 |

## 하나의 명령 실행 ##

### exec() ###

1. 결과가 String으로 저장되어 넘어오기 때문에 출력결과가 많으면 메모를 많이 사용하는 단점이 있다.

### execFile() ###

1. 실제로 shell을 실행하는 것이 아니기 때문에 windows에서 batch file을 실행할 경우는 "cmd.exe" command를 실행시키는 방식으로 실행하여야 한다.



## 병렬 프로그래밍 ##

### spawn() ###

### fork() ###

윈도우에서는 kill로 signal을 보낼 수 없다.
단, process.kill(), child_process.kill()은 무조건 죽이는 것으로 시뮬레이션된다. 이 때, child_process가 죽었는지 확인하기 위한 테스트용으로 child_process.kill()의 argument로 0을 사용할 수 있다.

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


