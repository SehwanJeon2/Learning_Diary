# Javascript 보충 2일차

[TOC]

## 주의

이번 수업은 질의 응답으로만 구성되어 있어서, 수업 내용의 두서가 없다.

내용 흐름이 다소 부자연스러울 수 있으며 일부 내용은 즉흥적으로 빠르게 진행되서 필기내용이 누락된 것이 많다.

누락된 부분을 수업 이후 기억을 되살려서 구현한 것이 제법 많다.

---

## this 보충

this란 것은 무엇인가?



python으로 돌아가서 class를 통해 비교하자.

**class.py** 

class method 구현하기.

```python
# class 객체 생성은 아래처럼 한다.
my = MyClass('ssafy')

# 왜 아래처럼 하지 않는가?
my.__init__('ssafy')


my = MyClass()  # memory 주소값을 할당한다.
# 실제로는 아래와 같이 동작
my = MyClass().__init__("ssafy")
```

그래서 우리의 추측 `.self` == `.this` 



**class.js**

함수가 받는 인자는 하나인데 key, value 쌍으로 추가 가능함. 왜냐하면 모든 것은 object 이기 때문에.

```javascript
class Car {
    constructor (name) {
        this.name = name;
    }

    introduce () {
        console.log(this.name)
    }
}

const car = new Car({name: 'hi'});
car.introduce()
// {name: "hi"}
```

마치 진짜 class처럼 구현하는 것 같지만, 실제로는 모두 object 형식으로 생성된다.

실제로 어떻게 돌아가는지는 강사님이 미리 준비한 **real.js** 통해 알아봤다.

미처 코드를 옮길 새가 없었지만, 정말 모든 구성이 object 형식으로 되어있었다.



**`new` keyword**

원래 new라는 것은 class를 위한 것이 아닌 함수를 위한 것.

애초에 class 개념은 ES6부터 나온 것으로 나온지 얼마 안된 것이다.

실제로는 JS class가 존재하지 않는다. 그래서 JS의 this랑 Python의 self랑 mapping 될 수 없는게 맞다. 하지만 비슷하게 작용한다는 것으로 이해해도 충분.



**JS에서는 모든게 객체, object**이라는 점을 다시 강조.



**일반적인 function 선언에서 this가 읽는 위치**

this의 핵심. 다른 key에 접근해서 value값을 뽑아오는 것이다. this는 같은 object에 있는 것을 뽑아오는 것이다. class 같은 거랑 같은 취급하면 안 된다.

아래 코드는 myObj 레벨 1 깊이에 내가 인위적으로 location을 추가했다.

```javascript
// self == this 비슷

const myObj = {
    name: 'ssafy',  // lv. 1
    age: 1,  // lv. 1
    introduce: function(){  // lv.1
        console.log(`Hi my name is ${this.name}, I'm ${this.age} years old`)
    },
    location: 'seoul',
    seoul: {  // lv. 1
        location: 'multicampus',  // lv. 2
        sayLocation: function () {  // lv. 2
            console.log(this.location)
        }
    }
};

myObj.introduce();
// Hi my name is ssafy, I'm 1 years old
myObj.seoul.sayLocation();
// multicampus
```

this는 같은 레벨 에서만 잡힌다. 여기서 this의 레벨은 function이 선언된 위치의 레벨까지 탐색 가능하다.

`console.log(this.location)` 에서 this는 sayLocation 함수와 같은 위치에 있는 'multicampus' 를 읽어온다.



**vue에서는 안 그러던데?**

같은 논리라면 vue를 사용한 객체에서는 methods 안에 있는 모든 get 함수는  data에 들어있는 name을 읽어올 수 없어야 한다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div id='app'>
        hi
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                name: 'hi'
            },
            methods: {
                get () {
                    console.log(this.name)
                },
                get1: function () {
                    console.log(this.name)
                },
                get2: () => {
                    console.log(this.document.title);
                }
            }
        })
        app.get();  // hi
        app.get1();  // hi
        app.get2();  // Document
    </script>
</body>
</html>
```

보다시피 `get()` 과 `get1()` 은 data.name을 읽어서 hi 라고 출력한다.

이것은 원래 object 였다면 읽을 수 없는 구조인데, vue가 알아서 붙여준 것이다. Vue를 구성하는 모든 object들을 분해했다 이어붙이는 과정에서 `.this` 를 통해 연결이 가능한 것이다.



혹시 vue 뿐만 아니라 원래 object도 가능한 것 아닌가?

```javascript
const hi = {
    el: 'asf',
    data: {
        name: 'hi'
    },
    methods: {
        say: function() {
            console.log(this.name)
        }
    }
}
hi.say()
// Uncaught TypeError: hi.say is not a function
//     at <anonymous>:1:4
hi.methods.say()
// undefined
```

vue와 구성을 같도록 했지만, 결국 this는 hi를 가져오지 못했다.





arrow function에서 this에서는 자기레벨이 아닌 한 단계 위를 보되, 함수 스코프를 본다.

함수 스코프를 본다는게 뭔가? (이것까지 설명하려면 너무 깊게 들어감)



**어벤저스 소개하기 with function key**

```javascript
const avengers = {
    members: ['Ironman', 'Hulk', 'Thor', 'Captain'],
    teamName: 'Avengers',
    teamSummary: function() {  // ['Ironman is Avengers', ...]
        return this.members.map(callbackfn = function(member) {
            return `${member} is ${this.teamName}`
        })
    }
}

console.log(avengers.teamSummary());
// ["Ironman is undefined", "Hulk is undefined", "Thor is undefined", "Captain is undefined"]
```

this.teamName이 Avengers를 못 잡는다.

오히려 애로우 함수로 구현해야 제대로 나온다.



**어벤저스 소개하기 with arrow function**

```javascript
const avengers = {
    members: ['Ironman', 'Hulk', 'Thor', 'Captain'],
    teamName: 'Avengers',
    teamSummary: function() {  // ['Ironman is Avengers', ...]
        return this.members.map( (member) => {
            return `${member} is ${this.teamName}`
        })
    }
}

console.log(avengers.teamSummary());
// ["Ironman is Avengers", "Hulk is Avengers", "Thor is Avengers", "Captain is Avengers"]
```



**this 문제로 절대로 이런 실수 안하려면, 아래 2가지를 외워라!**

일반적인 경우 - function keyword

콜백 함수인 경우 - arrow function



**arrow 함수는 익명함수**

arrow function 을 몰라서 질문 들어왔다.

일반 함수 선언과 python 이랑 비교해봄.

```javascript
function myFunc(name){
    return `hi ${name}`;
}
console.log(myFunc('ssafy'));
// hi ssafy

const otherFunc = (name) => `hi ${name}`
console.log(otherFunc('ssafy'));
// hi ssafy
```



```python
def myfunc(name) {
    return f'hi {name}'
}

otherFunc = lambda name: f'hi {name}'
print(other_func('ssafy'))
```





**일반함수, arrow fuction의 this 다시 비교**

arrow function의 this도 일반 함수의 this와 똑같이 출력할 수 있는 방법이 있다. arrow function의 this가 브라우저 기준으로 최상단인 window를 가리키는 것을 이용.

하지만 객체는 반드시 `var` 로 선언되어야 한다.

```javascript
var myObj = {
    title: 'hi',
    intro: function() {
        return this.title
    },
    outro: () => {
        return this.myObj.title
    }
}
```

var가 이러한 이리 저리 튀는 특성 때문에 단점도 많았고 많은 개발자들이 불만을 표현해왔다. 그래서 let, const가 나옴.



---

## 콜백함수 보충

인자로 넘기는 함수라는 뜻으로 이해하라.



JS는 선언만 하는게 가능하다.

```javascript
let number
// 초기값을 안 넣어도 괜찮다.
```





함수는 () 를 붙여야 실행된다.

그러니 넘겨주기만 하고 싶을 땐 괄호를 쓰지 말고, 함수를 실행하고 싶을 땐 괄호를 붙여야 한다.



---

## 코드 줄이는 Tip

**Syntatic Sugar (문법설탕)**

같은 기능을 유지하면서 더 짧고 간략한 코드로 구현 가능한 것을 의미.



**object 선언 줄여쓰기**

아래는 어떤 함수에 인자를 넣으면 object를 반환한다.

user라는 object를 선언할 때, key와 value가 같다.

```javascript
function saveUser(name, phone, email){
    const user = {
        name: name,
        phone: phone,
        email: email,
    }
    return user
}

person = saveUser("jeonse", "010", "jsws")
person.name
// "jeonse"
```

우리는 지금까지 key, value가 같은 object를 많이 선언해 왔었다. 이게 매우 귀찮아서 줄여쓰고 싶어했다.

그래서 아래처럼 줄이기가 가능하다.

```javascript
function saveUser(name, phone, email){
    const user = {
        name,
        phone,
        email,
    }
    return user
}

person = saveUser("jeonse", "010", "jsws")
person.name
// "jeonse"
```



**arrow function 줄여 선언**

참고로 인자 이름만 보면 더하기 같지만 빼기로 구현하셨다.

```javascript
myFunc = (n, sum) => {
    return sum - n
}
myFunc(1, 3)
// 2
```

애로우 함수에서 짧으면 중괄호, return문자 모두 생략 가능.

```javascript
myFunc = (n, sum) => sum - n
myFunc(1, 3)
// 2
```



---

Pycahrm은 JS에서 선언한 함수에 인자를 입력할 때, 함수에 들어가는 인자가 무얼 의미하는지 자동으로 가르켜 준다. 필기할 때 저 자동으로 뜬 것 때문에 많은 혼란이 일어났다.



## 추가 공부 - 참고한 사이트

<https://gomugom.github.io/is-class-only-a-syntactic-sugar/>

