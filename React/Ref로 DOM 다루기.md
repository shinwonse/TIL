# Ref로 DOM 다루기

```input```박스가 있는데 페이지를 로드할 때마다 자동으로 ```input```박스에 포커싱이 되게 하고싶다. 그렇다면 일단 단순하게 다음과 같은 방법을 생각할 수 있을 것이다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="root"></div>
    <script src="https://unpkg.com/react@17/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/babel">
      const rootElement = document.getElementById("root");
      const App = () => {
        React.useEffect(() => {
          document.getElementById("input").focus();
        }, []);

        return (
          <>
            <input id="input" />
          </>
        );
      };

      ReactDOM.render(<App />, rootElement);
    </script>
  </body>
</html>
```
하지만 ```id```를 직접 주는 방법은 주로 쓰지 않는다. 대신에 ```useRef```라는 ```React Hook```을 주로 사용한다. 예시는 다음과 같다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="root"></div>
    <script src="https://unpkg.com/react@17/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/babel">
      const rootElement = document.getElementById("root");
      const App = () => {
        const inputRef = React.useRef();
        React.useEffect(() => {
          inputRef.current.focus();
          // document.getElementById("input").focus();
        }, []);

        return (
          <>
            <input ref={inputRef} />
          </>
        );
      };

      ReactDOM.render(<App />, rootElement);
    </script>
  </body>
</html>
```
또 다른 예시로 Brown 컬러의 div를 1초후 Pink 컬러로 바꾸는 코드를 작성해보겠다.
```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="root"></div>
    <script src="https://unpkg.com/react@17/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/babel">
      const rootElement = document.getElementById("root");
      const App = () => {
        const inputRef = React.useRef();
        const divRef = React.useRef();
        React.useEffect(() => {
          inputRef.current.focus();
          // document.getElementById("input").focus();
          setTimeout(() => {
            divRef.current.style.backgroundColor = "pink";
          }, 1000);
        }, []);

        return (
          <>
            <input ref={inputRef} />
            <div
              ref={divRef}
              style={{ height: 100, width: 300, backgroundColor: "brown" }}
            />
          </>
        );
      };

      ReactDOM.render(<App />, rootElement);
    </script>
  </body>
</html>
```
그런데 왜 과연 React는 ```document.getElementById``` 류를 사용하지 않고 ```useRef```라는 별도의 방법을 제공할까?

리액트는 스스로 엘리먼트들을 최적화하는 그만의 로직을 갖고 있다. (ex. Virtual DOM) 그렇기 때문에 ```document.getElementById```를 통해 직접 엘리먼트에 도달하면 거기에서 비효율이 발생하는 것이다. 따라서 리액트가 관장하는 틀 안에서 무엇인가를 도달하게 하기 위해서 ```useRef```를 사용하는 것이다.
