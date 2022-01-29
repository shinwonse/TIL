로고가 멋있게 로딩되는 것도 구현할 수 있다.

![화면 기록 2022-01-29 오후 11 42 14](https://user-images.githubusercontent.com/62709718/151665278-3ead46aa-25ce-4f0a-b444-3411b39c08ea.gif)
```typescript
import styled from "styled-components";
import { motion } from "framer-motion";

const Wrapper = styled(motion.div)`
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
`;

const Svg = styled.svg`
  width: 300px;
  height: 300px;
  path {
    stroke: white;
    stroke-width: 2;
  }
`;

const svg = {
  start: { pathLength: 0, fill: "rgba(255, 255, 255, 0)" },
  end: {
    fill: "rgba(255, 255, 255, 1)",
    pathLength: 1,
  },
};

function App() {
  return (
    <Wrapper>
      <Svg
        xmlns="http://www.w3.org/2000/svg"
        focusable="false"
        viewBox="0 0 448 512"
      >
        <motion.path
          variants={svg}
          initial="start"
          animate="end"
          transition={{
            default: { duration: 5 },
            fill: { duration: 1, delay: 3 },
          }}
          d="M224 373.1c-25.24-31.67-40.08-59.43-45-83.18-22.55-88 112.6-88 90.06 0-5.45 24.25-20.29 52-45 83.18zm138.1 73.23c-42.06 18.31-83.67-10.88-119.3-50.47 103.9-130.1 46.11-200-18.85-200-54.92 0-85.16 46.51-73.28 100.5 6.93 29.19 25.23 62.39 54.43 99.5-32.53 36.05-60.55 52.69-85.15 54.92-50 7.43-89.11-41.06-71.3-91.09 15.1-39.16 111.7-231.2 115.9-241.6 15.75-30.07 25.56-57.4 59.38-57.4 32.34 0 43.4 25.94 60.37 59.87 36 70.62 89.35 177.5 114.8 239.1 13.17 33.07-1.37 71.29-37.01 86.64zm47-136.1C280.3 35.93 273.1 32 224 32c-45.52 0-64.87 31.67-84.66 72.79C33.18 317.1 22.89 347.2 22 349.8-3.22 419.1 48.74 480 111.6 480c21.71 0 60.61-6.06 112.4-62.4 58.68 63.78 101.3 62.4 112.4 62.4 62.89 .05 114.8-60.86 89.61-130.2 .02-3.89-16.82-38.9-16.82-39.58z"
        />
      </Svg>
    </Wrapper>
  );
}

export default App;
```
SVG 파일은 Awesome font에서 가져왔다.
