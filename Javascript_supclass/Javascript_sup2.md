# Javascript 보충 2일차

[TOC]

## this 보충

python으로 돌아가서 class 배워보자



class method



```python
my = MyClass('ssafy')

# 왜 아래는 아닌가?
my.__init__('ssafy')

# 아래
my = MyClass()  # memory 주소값을 할당한다.
# 실제로는 아래와 같이 동작
my = MyClass().__init__("ssafy")
```



그래서 우리의 추측

self == this



함수는 하나인데 키 벨류 추가 가능함. 왜냐하면 모든 것은 object 이기 때문에

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
```

마치 진짜 class처럼 구현하는 것 같지만, 실제로는 모두 object 형식으로 생성된다.

실제로 어떻게 돌아가는지는 real.js로 보여주겠다.



원래 new라는 것은 class를 위한 것이 아닌 함수를 위한 것.

class 개념은 나온지 얼마 안된 것이다.



실제로는 java class가 존재하지 않는다.

this랑 class랑 절대로 mapping 될 수 없다.



JS에서는 모든게 객체, object이라는 점을 다시 강조.



this의 핵심. 다른 key에 접근해서 value값을 뽑아오는 것이다. this는 같은 object에 있는 것을 뽑아오는 것이다. class 같은 거랑 같은 취급하면 안 된다.

```javascript
// self == this

const myObj = {
    name: 'ssafy',  // lv. 1
    age: 1,  // lv. 1
    introduce: function(){  // lv.1
        console.log(`Hi my name is ${name}, I'm ${age} years old`)
    },
    
    seoul: {  // lv. 1
        location: 'multicampus',  // lv. 2
        sayLocation: function () {  // lv. 2
            console.log(this.location)
        }
    }
};

myObj.introduce();
myObj.seoul.sayLocation();
```

this는 같은 레벨 에서만 잡힌다. 여기서 this의 레벨은 function이 선언된 위치의 레벨까지 탐색 가능하다.

function 안에서의 this는 내 레벨 안에서 밖에 못 논다.



```javascript
const hi = {
    el: 'asf',
    data: {
        name: 'hi'
    },
    methods: {
        say: function() {
            console.log(this.location)
        }
    }
}
```





arrow function에서 this에서는 자기레벨이 아닌 한 단계 위를 보되, 함수 스코프를 본다.

함수 스코프를 본다는게 뭔가?





어벤저스

```javascript
const avengers = {
    members: ['Ironman', 'Hulk', 'Thor', 'Captain'],
    teamName: 'Avengers',
    teamSummary: function() {  // ['Ironman is Avengers', ...]
        return this.members.map(callbackfn: function(member) {
            return `${member} is ${this.teamName}`
        })
    }
}

console.log(avengers.teamSummary());
```

this.teamName이 Avengers를 못 잡는다.

오히려 애로우 함수로 구현해야 제대로 나온다.





절대로 이런 실수 안하려면,

일반적으로는 그냥 함수

콜백 함수만 애로우 함수 쓰면 된다.



애로우 함수의 this는 자기 함수 밖



```javascript
function myFunc(name){
    return `hi ${name}`;
}

const otherFunc = (name) => `hi ${name}`
console.log(otherFunc(name: 'ssafy'));
```



```python
def myfunc(name) {
    return f'hi {name}'
}

otherFunc = lambda name: f'hi {name}'
print(other_func('ssafy'))
```







브라우저 기준은 window가 최상단.



window.document === myObj.outro()

true



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

var를 써야만 했다.

이게 이리 저리 튀어서 단점도 많은데, var let const가 나옴.



---

## 콜백함수 보충

인자로 넘기는 함수라는 뜻으로 이해하라.



JS는 선언만 하는게 가능하다.



함수는 () 를 붙여야 실행된다.

그러니 넘겨주기만 하고 싶을 땐 괄호를 쓰지 말고, 쓰고 싶을 땐 괄호를 사용한다.



---

## 코드 줄이는 Tip

sugar syntax라는 것은 문법을 간략하게 하려는 것.





애로우 함수에서 짧으면,

중괄호, return문자 모두 생략 가능.

```javascript
myFunc: (n, sum) => sum - n
```



---

Pycahrm은 함수에 들어가는 인자가 무얼 의미하는지 자동으로 가르켜 준다.

