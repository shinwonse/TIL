**const**는 ES6 문법에서 새로 도입되었으며 한번 지정하고 나면 변경이 불가능한 상수를 선언할 때 사용하는 키워드입니다. **let**은 동적인 값을 담을 수 있는 변수를 선언할 때 사용하는 키워드입니다.

ES6 이전에는 값을 담는 데 var 키워드를 사용했었습니다. var 키워드는 scope가 함수 단위입니다.

```javascript
function myFunction() {
  var a = "hello";
  if(true) {
    var a = "bye";
    console.log(a); // bye
  }
  console.log(a); // bye
}
myFunction();
```
if문 밖에서 var 값을 hello로 선언하고, if문 내부에서 bye로 설정했습니다. if문 내부에서 새로 선언했음에도 불구하고 if 문 밖에서 a를 조회하면 변경된 값이 나타납니다.

이런 결점을 해결해 주는 것이 바로 let과 const입니다.
```javascript
function myFunction() {
  let a = 1;
  if(true) {
    let a = 2;
    console.log(a); // 2
  }
  console.log(a); // 1
}
myFunction()
```
let과 const는 scope가 함수 단위가 아닌 **블록 단위이므로**, if 문 내부에서 선언한 a 값은 if 문 밖의 a 값을 변경하지 않습니다.

그렇다면 어떤 상황에 각 키워드를 사용해야 할까요? 일단 ES6 문법에서 var을 사용할 일은 없습니다. let은 한번 선언한 후 값이 유동적으로 변할 수 있을 때만(예: for문) 사용하고, const는 한번 설정한 후 변할 일이 없는 값에 사용합니다.

편하게 생각하면 기본적으로는 **const**, 해당 값을 바꿔야 할 때는 **let** !!
    
