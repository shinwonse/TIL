## MotionValues
드래그의 위치에 따라 효과를 주고 싶다면 다음과 같은 코드를 작성해보는 것도 좋다.

```typescript
import styled from "styled-components";
import { motion, useMotionValue, useTransform } from "framer-motion";
import { useEffect } from "react";

const Wrapper = styled.div`
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
`;

const Box = styled(motion.div)`
  width: 200px;
  height: 200px;
  background-color: rgba(255, 255, 255, 1);
  border-radius: 40px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

function App() {
  const x = useMotionValue(0); // ReactJS 세계에 없음. 재렌더링 x
  const scale = useTransform(x, [-800, 0, 800], [2, 1, 0.1]);
  useEffect(() => {
    scale.onChange(() => console.log(scale.get())); // framer motion function 임 onChange, get, set...
  }, [x]);
  return (
    <Wrapper>
      <Wrapper>
        <Box style={{ x, scale: scale }} drag="x" dragSnapToOrigin />
      </Wrapper>
    </Wrapper>
  );
}

export default App;
```

![화면 기록 2022-01-29 오후 10 18 25](https://user-images.githubusercontent.com/62709718/151662588-1d24e638-7313-4f13-b1dd-33e3663d24c6.gif)

아래와 같이 x축으로 드래그하면 배경색이 바뀌는 효과도 구현할 수 있다.
```typescript
import styled from "styled-components";
import { motion, useMotionValue, useTransform } from "framer-motion";

const Wrapper = styled(motion.div)`
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
`;

const Box = styled(motion.div)`
  width: 200px;
  height: 200px;
  background-color: rgba(255, 255, 255, 1);
  border-radius: 40px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

function App() {
  const x = useMotionValue(0); // ReactJS 세계에 없음. 재렌더링 x
  const rotateZ = useTransform(x, [-800, 800], [-360, 360]);
  const gradient = useTransform(
    x,
    [-800, 0, 800],
    [
      "linear-gradient(135deg, rgb(0, 210, 238), rgb(0, 83, 238))",
      "linear-gradient(135deg, rgb(238, 0, 153), rgb(221, 0, 238))",
      "linear-gradient(135deg, rgb(0, 238, 155), rgb(238, 178, 0))",
    ]
  );
  return (
    <Wrapper style={{ background: gradient }}>
      <Box style={{ x, rotateZ: rotateZ }} drag="x" dragSnapToOrigin />
    </Wrapper>
  );
}

export default App;
```

![화면 기록 2022-01-29 오후 10 39 12](https://user-images.githubusercontent.com/62709718/151663353-ebe2af9e-faba-40f8-930c-89e2122ad743.gif)

스크롤을 오르락 내리락하는거에 따른 효과도 줄 수 있다.
```typescript
import styled from "styled-components";
import {
  motion,
  useMotionValue,
  useTransform,
  useViewportScroll,
} from "framer-motion";

const Wrapper = styled(motion.div)`
  height: 200vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
`;

const Box = styled(motion.div)`
  width: 200px;
  height: 200px;
  background-color: rgba(255, 255, 255, 1);
  border-radius: 40px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

function App() {
  const x = useMotionValue(0); // ReactJS 세계에 없음. 재렌더링 x
  const rotateZ = useTransform(x, [-800, 800], [-360, 360]);
  const gradient = useTransform(
    x,
    [-800, 0, 800],
    [
      "linear-gradient(135deg, rgb(0, 210, 238), rgb(0, 83, 238))",
      "linear-gradient(135deg, rgb(238, 0, 153), rgb(221, 0, 238))",
      "linear-gradient(135deg, rgb(0, 238, 155), rgb(238, 178, 0))",
    ]
  );
  const { scrollYProgress } = useViewportScroll();
  const scale = useTransform(scrollYProgress, [0, 1], [1, 5]);
  return (
    <Wrapper style={{ background: gradient }}>
      <Box
        style={{ x, rotateZ: rotateZ, scale: scale }}
        drag="x"
        dragSnapToOrigin
      />
    </Wrapper>
  );
}

export default App;
```
![화면 기록 2022-01-29 오후 10 51 24](https://user-images.githubusercontent.com/62709718/151663637-22c45d9e-99fa-4ada-92d1-595c90727149.gif)
