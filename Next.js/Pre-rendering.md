![](https://images.velog.io/images/shinwonse/post/9e448489-6087-4d70-975e-70a97be8167b/image.png)
## Pre-rendering
***기본적으로 Next.js***는 모든 페이지를 ```pre-renders```한다. 이 말은 무슨 말이냐면 ```client-side JavaScript```가 페이지의 HTML을 다 그리게 놔두는 것이 아니라 미리 각 페이지를 위한 HTML을 생성한다는 뜻이다.

각각의 생성된 HTML은 최소한의 자바스크립트와 연관되어 있다. 페이지가 브라우저에 의해 로드되었을 때, 자바스크립트가 페이지를 완전히 완성시킨다. (이 과정을 ```hydration```이라고 한다.)

## Two forms of Pre-rendering
Next.js는 두 가지의 ```pre-rendering``` 형태를 가지고 있다. 바로 ```Static Generation```과 ```Server-side Rendering```이다. 이 둘의 차이점은 HTML을 생성할 때에 있다.

- Static Generation (Recommended): The HTML is generated at ***build time*** and will be reused on each request
- Server-side Rendering: The HTML is generated on ***each request***

공식 문서에는 ```Static Generation```에 Recommended로 명시가 되어있다. 그 이유는 성능 문제 때문이라고 한다. 하지만 사용자는 이 둘을 자유롭게 섞어쓸 수 있다. Next.js는 대부분의 페이지를 ```Static Generation```을 통해 구현하고 그 나머지를 ```Server-side Rendering```으로 구현할 것을 제안한다.

```Client-side Rendering``` 또한 함께 사용할 수 있다. 그 말인즉슨, 페이지의 일부분은 ```client side JavaScript```에 의해 렌더링될 수 있다는 뜻이다. 아마 사용자의 선택에 따른 ```Data Fetching```의 경우가 아닐까라고 생각이 든다.
