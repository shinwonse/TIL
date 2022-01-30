Framer Motion은 리액트 요소들의 변화를 모두 animate 해줄 수 있다. 아래와 같은 예시가 있다.

```typescript
import styled from "styled-components";
import { AnimatePresence, motion } from "framer-motion";
import { useState } from "react";

const Wrapper = styled(motion.div)`
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: space-around;
  align-items: center;
`;

const Box = styled(motion.div)`
  width: 400px;
  height: 400px;
  background-color: rgba(255, 255, 255, 1);
  border-radius: 40px;
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

const Circle = styled(motion.div)`
  background-color: #00a5ff;
  height: 100px;
  width: 100px;
  border-radius: 50px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

function App() {
  const [clicked, setClicked] = useState(false);
  const toggleClicked = () => setClicked((prev) => !prev);
  return (
    <Wrapper onClick={toggleClicked}>
      <Box
        style={{
          justifyContent: clicked ? "center" : "flex-start",
          alignItems: clicked ? "center" : "flex-start",
        }}
      >
        <Circle layout />
        {/* layout prop은 엘리먼트의 변화를 애니메이트해준다. */}
      </Box>
    </Wrapper>
  );
}

export default App;
```
css 스타일이 변화되는 엘리먼트 Circle에 ```layout``` 이라는 prop 하나만 넣었을 뿐인데 Circle의 변화를 animate 해주는 모습을 볼 수 있다.
![화면 기록 2022-01-30 오후 7 10 43](https://user-images.githubusercontent.com/62709718/151695474-968f1495-7647-4271-8d36-93fc06b8ab9d.gif)

더 신기한 일도 할 수 있다. 분명 서로 다른 컴포넌트인데 ```layoutId="cicle"``` 이라고 Id를 통일시켜주니까 아래와 같은 애니메이션을 얻을 수 있다.
```typescript
import styled from "styled-components";
import { AnimatePresence, motion } from "framer-motion";
import { useState } from "react";

const Wrapper = styled(motion.div)`
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: space-around;
  align-items: center;
`;

const Box = styled(motion.div)`
  width: 400px;
  height: 400px;
  background-color: rgba(255, 255, 255, 1);
  border-radius: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

const Circle = styled(motion.div)`
  background-color: #00a5ff;
  height: 100px;
  width: 100px;
  border-radius: 50px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

function App() {
  const [clicked, setClicked] = useState(false);
  const toggleClicked = () => setClicked((prev) => !prev);
  return (
    <Wrapper onClick={toggleClicked}>
      <Box>{!clicked ? <Circle layoutId="circle" /> : null}</Box>
      <Box>{clicked ? <Circle layoutId="circle" /> : null}</Box>
    </Wrapper>
  );
}

export default App;
```
![화면 기록 2022-01-30 오후 7 18 01](https://user-images.githubusercontent.com/62709718/151695717-1d0c962e-9e85-4148-9d27-1ed2c45c0af4.gif)


