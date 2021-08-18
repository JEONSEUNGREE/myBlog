---
title: JS클래스
date: 2021-08-01 20:33:58
tags:
  - javascript
  - vue
categories:
  - javascript
---

JS에서도 클래스가 가능하다.
-----------클래스 선언------------
constructor(name,age) {
this.name = name
this.age = age
}
speak() {
console.log(`${this.name}: Hello!`);
}
}

-----------인스턴스 생성------------

const ree = new PerSon('ree',28);
console.log(ree.name)
console.log(ree.age)
ree.speak();

-----------Static사용 예------------

Class Article {
	static publisher = "coding";
}
const article = new Article();

console.log(Article.publisher) O
console.log(article.publisher) X

static :  Object마다 할당X 클래스 자체에 있음

-----------상속가능(Interitance)------------


class Shape {
constructor(width, height, color) {
this.width = width;
this.heigth = height;
}
getArea() {
return width * this.heigth;
}
}
class Recteangle extends Shape {}

const rectangle = new Rectangle( 20, 20);
rectangle.getArea();

instance of 도있다. 자바랑 매우 비슷하다.

출처는 드림코딩 by 엘리 강의를 보고 정리했다.