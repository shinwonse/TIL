## Drag
![스크린샷 2022-01-29 오후 9 27 57](https://user-images.githubusercontent.com/62709718/151661012-7379db76-7af3-4fd6-ad23-543358870aaa.png)

framer-motion 의 해당 예제를 해보도록 하겠다.

```typescript
import styled from "styled-components";
import { motion } from "framer-motion";
import { useRef } from "react";

const Wrapper = styled.div`
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
`;

const BiggerBox = styled.div`
  width: 600px;
  height: 600px;
  background-color: rgba(255, 255, 255, 0.4);
  border-radius: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
`;

const Box = styled(motion.div)`
  width: 200px;
  height: 200px;
  background-color: rgba(255, 255, 255, 1);
  border-radius: 40px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

const boxVariants = {
  hover: { scale: 1, rotateZ: 90 },
  click: { scale: 1, borderRadius: "100px" },
  drag: { backgroundColor: "rgb(46,204,113)", transition: { duration: 5 } },
};

function App() {
  const biggerBoxRef = useRef<HTMLDivElement>(null);
  return (
    <Wrapper>
      <BiggerBox ref={biggerBoxRef}>
        <Box
          drag // 드래그 가능
          dragSnapToOrigin // 드래그해도 제자리로 돌아감
          dragElastic={0.5} // 드래그가 잡아당기는 힘
          dragConstraints={biggerBoxRef} // 드래그 제약
          variants={boxVariants}
          whileHover="hover"
          whileDrag="drag"
          whileTap="click"
        />
      </BiggerBox>
    </Wrapper>
  );
}

export default App;
```

***결과화면***
![화면 기록 2022-01-29 오후 9 29 14](https://user-images.githubusercontent.com/62709718/151661085-97c71213-1ede-4a13-8561-a6636111ddf7.gif)

