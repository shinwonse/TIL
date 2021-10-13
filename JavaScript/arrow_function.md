화살표 함수(arrow function)는 ES6 문법에서 함수를 표현하는 새로운 방식입니다. 그렇다고 해서 기존 function을 이용한 함수 선언 방식을 아예 대체하지는 않습니다. 사용 용도가 조금 다릅니다. 이 문법은 주로 함수를 파라미터로 전달할 때 유용합니다.
```javascript
setTimeout(function() {
  console.log('hello world');
}, 1000);

setTimeout(() => {
  console.log('hello world');
}), 1000);
```
이 문법이 기존 function을 대체할 수 없는 것은 용도가 다르기 때문입니다. 무엇보다 서로 가리키고 있는 this 값이 다릅니다.

```javascript
function BlackDog() {
  this.name = '흰둥이';
  return {
    name: '검둥이',
    bark: function() {
      console.log(this.name + ': 멍멍!');
    }
  }
}

const blackDog = new BlackDog();
blackDog.bark(); // 검둥이: 멍멍!

function WhiteDog() {
  this.name = '흰둥이';
  return {
    name: '검둥이';
    bark: () => {
      console.log(this.name + ': 멍멍!');
    }
  }
}

const whiteDog = new WhiteDog();
whiteDog.bark(); // 흰둥이: 멍멍!
```
function()을 사용했을 때는 검둥이가 나타나고, () =>를 사용했을 때는 흰둥이가 나타납니다. **일반 함수는 자신이 종속된 객체를 this로 가리키며, 화살표 함수는 자신이 종속된 인스턴스를 가리킵니다.**

화살표 함수는 값을 연산하여 바로 반환해야 할 때 사용하면 가독성을 높일 수 있습니다.

```javascript
function twice(value)  {
  return value * 2;
}

const triple = (value) => value * 3;
```
이렇게 따로 { }를 열어주지 않으면 연산한 값을 그대로 반환한다는 의미입니다.