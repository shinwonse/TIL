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
