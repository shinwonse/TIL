![image](https://user-images.githubusercontent.com/62709718/148504579-7054e819-391d-4ede-a37f-6db7a9a5ad70.png)

자바스크립트나 타입스크립트의 입장에서 데이터를 주고 받을 때 ```객체```를 주고받을 수 있으면 가장 효과적일 것이다. 하지만 객체는 자바스크립트의 실행하는 시간에 메모리 상에만 존재하는 구조이기 때문에, 데이터로써 주고 받기가 어렵다.

이를 해결하기 위해 데이터를 주고받기 위해 개발된 포맷이 바로 ```JSON```이라는 포맷이다.

```json
const jsonString = `
  {
    "name": "Shin won se",
    "age": 27,
    "bloodType": "A"
  }
`;
```
```JSON```은 데이터를 주고받기 위한 용도로 만들어진 포맷이기 때문에 데이터 타입, ```value```의 종류가 제한적이다. 문자열, 숫자, 배열, boolean, 그리고 객체 타입을 지원한다.

```JSON```데이터를 실제로 자바스크립트에서 사용하기 위해서는 객체로 변환하는게 필요하다. 자바스크립트는 JSON 객체라고 하는 내장 객체를 제공한다. 이것을 써서 간단하게 객체로 변환시킬 수가 있다.

```javascript
// JSON 내장 객체의 parse 메서드를 활용하여 JSON 데이터를 객체 형태로 변환
const myJson = JSON.parse(jsonString);

console.log(myJson.name); // 'Shin won se'

// myJson 객체의 데이터를 전송하기 위해서 JSON으로 만들고 싶다
// JSON.stringify에 객체를 넣어주면 JSON 문자열로 리턴해줌
console.log(JSON.stringify(myJson)) // '{"name":"Shin won se", "age":27,"bloodType":"A"}'
```
위와 같이 리턴된 JSON 문자열을 서버에 전송하거나 DB에 저장할 수 있다. 다시 꺼내올 때는 꺼내와서 ```JSON.parse```로 객체로 변환한 다음에 쓰고, 업데이트하고, 그런 다음에 다시 저장할 땐 ```JSON.stringify```로 문자열로 만든 다음에 저장한다.

여기서 주의할 점은 자바스크립트의 객체는 함수를 지원하지만 JSON은 데이터이기 때문에 함수를 지원하지 않는다. 또, 만약 JSON 데이터에 문제가 있다면 어떤 일이 발생할까 예를 들어 double quote이 아닌 single quote같은 경우 말이다. 이럴 경우에는 서비스가 멈추게 되어 사용자의 경험이 나빠지게 된다. 이런 경우를 대비하여 이에 맞춘 화면을 준비한다면 훨씬 좋은 UI가 될 것이다.

```JSON.parse```를 할 때 발생하는 오류는 예외로 발생하기 때문에 ```try catch```구문으로 잡을 수가 있다.
```javascript
try{
  const myJson = JSON.parse(jsonString);
  
  console.log(myJson.name);
  
  console.log(JSON.stringify(myJson));
} catch(e) {
  console.log('다시 한번 시도해 주세요.');
}
```
항상 이렇게 JSON.parse를 할 때는 혹시 모를 이런 오류 상황을 위해서 항상 ```try-catch``` 예외 구문으로 감싸서 오류 상황에 대비를 해야 한다.
