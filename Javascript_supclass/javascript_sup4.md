> # 2019.05.09

# Javascript 보충 4일차

지금까지 axios를 써보기 위해 다른걸 배웠다면, 이제는 다른 걸 쓰기 위해 axios를 활용하는 느낌으로 전환될 때다.



노드와 브라우저로 나뉨



그리고 노드 중에서 non-blocking



일과?들이 



배가 고프면(addEventListener) (뭘 한다 - 콜백함수)

JS 입장에서는 콜백함수가 뭐가 있는지 궁금해하지도 않는다. 그래서 배가 고픈것만 확인하자마자 쌩깐다.

그래서 배고픔의 콜백함수가 끝나면 걔가 찾아와서 콜백함수에 대한 적용. 원래 할려던 말이 뭐였어? 아~ 너 때릴라고~!

그래서 nonblocking 실행되면 걔 자체는 Unknow 상태라고 불린다. 콜백함수가 완료되기 전까지 어떤일이 일어날지 몰라서. 마치 카톡을 열기 전까지 어떤 문자가 온건지 모르는 것과 마찬가지. 게다가 핸드폰을 잃어버릴수도 있고 배터리도 나가버리면 영원히 모를수도 있다.

포인트는 내 밖에서 일어난 일은 알 수 없고 그러한 모든 기능은 비동기적으로 일어난다. 시간이 얼마나 걸리는지도 모른다.

같은 코드도 컴퓨터 성능에 따라서 연산이 완료되는 시간이 다를 수 있다. 그러니...



폼은 10만개 돌리면 3~4초 정도 걸린다. 하지만 JS 안에서 일어나는 일이라면 JS는 얼마나 걸리는지는 몰라도 내가 알 수 있다고 확신하여, 그 함수가 완료될 때까지 기다린다. 실제로 얼마나 오래 걸리는지 상관없이.





시간 멈추기, data I/O 밖에 없다. 이 2개밖에 없다. (내 밖에서 일어난 일이라는게)

node JS 유명한 개발자의 말 "지금 데이터 처리는 굉장히 잘못 됐다." "we all doing it wrong" data I/O는 잘못사용하고 있기 때문에, 자신이 node js를 들고왔다고 말한다.

(그 개발자의 연설이 인상깊다. Ryan Dahi: Original Node.js presentation)





실제로 그렇다고한다. (1)web/network에서 data 보내서 (2) Data I/O로 보내짐? 얘네들은 파일 하나만 딸랑 있는게 아니다. Django는 얘가 하는 일이 있고, MYSQL은 얘가 하는 일이 있고, 이 둘 사이에서 통신이 일어나야한다. 하지만 지금 우리가 한것은 그런 과정이 없었다.

그래서 우리는 비동기가 2개 밖에 없는것, 시간 멈추기랑, Axios 끝~ (fetch 등은 우리가 안 쓰기로 했으니까)

data 과 web req res



**보충 설명**

식당으로 치면 기존에는 종업원이 주문받아 주방에 요청하고 요리가 나올 때까지 그냥 기다려서 손님이 밀린다.

하지만 요즘것은 주문받아 주방에 요청하고 다음 손님 주문을 받으로 움직이는 방식으로 처리한다.

참고영상 how node js work moah (유튜브)



많은 사람이 착각하는게 sqlite3 쓰는 사람은 db는 엑셀이라고 착각하지만, 실제로는 프로그램이다. 가전제품이 콘센트 꽂아야 작동하는 것과 같다.

DB가 뻗어버리는 것과 서버가 뻗어버리는 것과 다르다. 장고가





우리도 곧 고통받게 될 건데, 내 코드 중에서 뭘 쓰는걸 보니까, 내가 망할거란 것 정도는 미리 알 수 있다.

우리 DB 안건드리고 시간도 안 멈출거다.

axios.get(), axios.post() 를 쓸건데, 얘가 등장한 것만으로도 우린 미리 마음의 준비를 할 수 있다.



Python과 JS가 다르다고 체감한 포인트가 있는가?

3반 강사님입장에선, 함수화를 굉장히 많이 해야겠구나를 느끼셨다고 함.

파이썬은 실제로 for 써도 함수화 시킨뒤 return을 항상 붙이지는 않았다.

JS에서는 내가 하고싶은 기능들을 대부분 하나로 묶어서 사용했다.

(여담인데, 함수형 프로그래밍과는 달라???)



axios.get()이 파이썬 입장에선 죽었다 깨어나도 못 해결한다.

그래서 3가지 방법을 동원.

1. ~하면 ~하자! 이게 보통이지만, JS는 ~하면 여기서 끝나서 ~하자! 는 세상에 등장하지 못한다.

   비동기 안에 그냥 함수 넣어서 해결하려 했다. 그러다보니 콜백헬이 생김.

   그러다가 Promise가 등장했고 표준화로 채택됨.

   promise 등장으로 먼저 바뀐 것은. 함수 안에 axios 같은 비동기 넣어도 return 가능해짐. 기전에는 callback(값) 을 해왔었다.

   그리고 체이닝이 가능해졌다. promise가 promise를 강제로 뱉으니 .then 여러개로  new Promise 없이 사용했다.

   여전히 사람들이 고통스러워했다.

   ```python
   data = requests.get()
   value = data[0]
   sss = value
   ```

   이런 작업은 여전히 못한다. 그래서

   1000줄 짜리 함수가 있다고 하자. JS는 이걸 자신이 해결할 수 있다고 믿기에 함수가 완료될 때까지 기다리려고 한다. 그 중에서 1줄이라도 비동기가 들어가면, 함수 전체를 비동기함수라고 할 수 있다. 1000줄 중 하나의 함수 안에 숨겨져 있어도 마찬가지. 정말로 전체 중에서 아~무것도 비동기가 없어서 그 함수는 비동기가 아니다.

   async는 비동기함수라면 무조건 붙인다. 마치 독극물이 포함되어 있다면 경고스티커 붙이듯이. async?! 너 위험한 애야! 그리고 axios 같은걸 발견하면 "찾았다 요놈!"

   ```javascript
   async function something () {  // async 붙이게 한 원인이 있다!
       const a = 1
       const data = await axios.get()  // 요놈이 원흉
   }
   ```



여기서 우리가 코드가 변하는 걸 공식처럼 알려줄 것이다.

axios 로 얘기해보겠다.

```javascript
axios.get(url)  // 응답을 받는다
data.parse()  // 가공한다
pushtoDOM()  // 보여준다
// 이게 70% 정도

// 반대편으론
post  // 요청을 보낸다
O, X (성공, 실패)  // 응답을 받는다
실패하면 다시 시도 등 자유로운 핸들링  // ok
```

위 2단계 할라고 JS 배우고 있는 것과 마찬가지.

```javascript
const data = axios.get(url)
	.then( res => {
        const data = res.data  // 이건 의미가 없으니
        return res.data  // 이렇게 보내자
    })  // 하지만 res.data는 바로 못 쓴다. 왜냐하면 강제로 Promise라서
	.then( d => DOM(d))
```

일단 then으로 코드 짠 뒤에 해석하자. 누가 누구랑 매핑 되는지. res는 axios.get(url)이랑, d 는 res.data랑 mapping.

이래 보여도 쓰기 편하게 만들어 놓은 것 뿐이지, 실질적인 코드가 바뀐 것이 아니다.



1. axios 코드 일단 then으로 짠다.
2. axios ~ 마지막 then 까지 함수 하나로 묶는다.
3. 얘는 취급 주의해야하는 함수니까 async func 으로 바꾼다.
4. async func 붙어있는 상태애서 axios를 찾는다. 그걸 const 등으로 받는다고 생각하면 된다. 미래지향적이 것에서 현재 진행형으로 바라봐야 한다.
5. 앞에 await를 붙인다.

아쉽게도 .then, .catch로 에러 핸들링 안된다?

그래서 현재 진행형 애들을 모두 try로 묵고 마지막에 catch



promise를 .then에서 async await로 바꾸는 5단계 + 알파



```javascript
// 1단계 : then 으로 코드를 짠다.
axios.get(url)
        .then(response => { return JSON.parse(response)})  // beautify(response) 도 상관없다 어차피 이름 따위야 자유
        .then(goodData => { return goodData.split() })
        .then(goodArray => { return goodArray.join('-') })
        .then(hypenData => { pushToDom(hypenData) })  // 실제로 없는 함수지만 뭐...
        .catch( badOne => { console.error(badOne)})   // 말만 들어보면 미래 지향적으로 말하게 된다.

// 2. 함수로 묶는다.
function oddFunc() {
    axios.get(url)
        .then(response => { return JSON.parse(response)})
        .then(goodData => { return goodData.split() })
        .then(goodArray => { return goodArray.join('-') })
        .then(hypenData => { pushToDom(hypenData) })
        .catch( badOne => { console.error(badOne)})
}

// 3. 비동기 주의! async 키워드를 붙인다.
async function oddFunc() {
    axios.get(url)
        .then(response => { return JSON.parse(response)})
        .then(goodData => { return goodData.split() })
        .then(goodArray => { return goodArray.join('-') })
        .then(hypenData => { pushToDom(hypenData) })
        .catch( badOne => { console.error(badOne)})
}

// 4. 오늘을 산다. 위는 미래를 사는 느낌.
async function oddFunc() {
    const response = axios.get(url)
    const goodData = JSON.parse(response)
    const goodArray = goodData.split()
    const hypenData = goodArray.join('-')
    pushToDom(hypenData)
}

// 5. 비동기 함수앞에 await를 붙인다.
async function oddFunc() {
    const response = await axios.get(url)
    const goodData = JSON.parse(response)
    const goodArray = goodData.split()
    const hypenData = goodArray.join('-')
    pushToDom(hypenData)
}

// 6. 에러를 잡는다.
async function oddFunc() {
    try {
        const response = await axios.get(url)
        const goodData = JSON.parse(response)
        const goodArray = goodData.split()
        const hypenData = goodArray.join('-')
        pushToDom(hypenData)
    } catch(badOne) {
        console.error(badOne)
    }
}
```









Django API 랑 Vue SPA는 완전히 별도의 앱

Vue SPA가 Django에게 요청을 보냈다. 

get, url(uniform resource location)

처음 만나게 되는 것은 urls.py 일거다. 그 다음 views.py => models.py => DB => models.py => views.py => templates(?) NO! 그냥 JSON을 Vue SPA로 보내줌.



Vue SPA는 사용하고 있는 사람이 누구인지는 알지도 않고 알고 싶지도 않아한다. 그냥 하라는 데로 요청하는 일 밖에 없다.

사용자가 누구인지 검증하는 것은 django가 해줘야 한다. 근데 이전에는 templates에서 바로 왔다갔다해서 검증하기가 굉장히 편했는데, 이젠 서로 다른 영역에서 왔다갔다 해야하니 검증이 굉장히 까다로워졌다.

그래서 누군가에게 대신 맡기겠다!

이제는 쿠키로 모든 사용자 인증을 할 수 없게 되었다. 쿠키는 templates와 DB가 왔다갔다 할 때나 의미있지, vue django 사이에는 의미없고 위험하다.

쿠키는 브라우저가 세팅해줄 수 있었다. 그래서 애네둘이 한 몸일 때는 직접 접근해서 세팅이 가능한데, vue django 2개로는 안된다.

JWT라는게 있다. JSON WEB TOKEN. 쿠키랑 동작원리는 똑같지만 만드는게 조금 더 어렵다.

근데 이것도 직접 만드는 거라 좀 버거우면, 다른 것에게 맡기는 방법이 있다.



옛날에는 로직에서 벗어나지 않는 것이라면 없는 주민등록번호도 통과시킬 수 있었다. 주민등록번호 생성기가 있기도 했었다.

버겁다 보니 요즘은 본인인증을 핸드폰 번호로 한다. 근데 이걸 핸드폰 번호가 본인이랑 맞는지 사이트가 직접하지 않고 통신사가 해준다.

그러다보니 사이트 DB가 털려도 회원의 개인정보는 털릴 일이 없다. 왜냐면 자기가 가지고 있을 필요 없으니까.

OAuth라고 하니 참고.

이걸 썼을 때 회원 유입률이 굉장히 좋아진다. 왜냐하면 개인정보를 거의 안적으면서 가입이 가능하기 때문이다. 요새 이메일, 비밀번호 가지고 가입하라하면 잘 안함. 그냥 카카오 등을 통해 버튼 몇 번만 누르면 바로 가입 완료되게 해주니 겁나 편하다.

페이스북이 구글이랑 비슷하다. 카카오톡은 좀...

JWT는 공부할 때는 가치가 있다. 하지만 당장 서비스하면서 재미좀 보려면 OAuth만 해도 상관없다.





우와 드디어 첫 번째 방법이 끝났다!!!!!!!!!!! 응??????????



## 2. 프로젝트 제작 방법

2가지 방법이 있다.

따로 만들거나 장고안에 cdn으로 js 집어넣는다.

1번의 어렵지만 확장성이 좋다. 서버를 따로 만드니 클라이언트를 vue 말고 다른걸로 고쳐 쓰기 편하다.

2번은 만들기 쉽고 남은 시간을 서버 만들기에 집중할 수 있다. 하지만 확장성이 낮다.



두 번째 방법은 Django다.

한 달 전까지 만해도 실컷 했던것. req 받으면 urls => models => views => res 해준다.

평소 하던 것에 JS(CDN) 한 방울 톡 떨어뜨리면 끝이다.



어 갑자기 android 만들고 싶다, tyzen, i8, react, tl8? 로 만들어지고 싶다면, 내부 다 뜯어고쳐야 한다.

근데 ~를 기반으로하면 그나마 확장성이 원활하게 할 수 있다.

vue 대신 쓰고 싶은게 생길 대 이 방법이 유용하다.





프로젝트 할 때 고민의 흐름은 아래와 같다.

~가 뭔가? modeling

고민은 모델 다 짜고나면 머리가 url로 간다. 어차피 CRUD 방법이야 매번 하던거니까 그건 별로 부담스럽지 않는데, url은 개수 x4배 해줘야 하니까...



