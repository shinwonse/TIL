***atom.tsx***
```typescript
import { atom, selector } from "recoil";

export const minuteState = atom({
  key: "minutes",
  default: 0,
});

export const hourSelector = selector<number>({
  key: "hours",
  get: ({ get }) => {
    const minutes = get(minuteState);
    return minutes / 60;
  },
  set: ({ set }, newValue) => {
    const minutes = Number(newValue) * 60;
    set(minuteState, minutes);
  },
});

```

***App.tsx***
```typescript
import React from "react";
import { useRecoilState } from "recoil";
import { hourSelector, minuteState } from "./atoms";

function App() {
  const [minutes, setMinutes] = useRecoilState(minuteState); // useRecoilState를 atom으로 사용
  const [hours, setHours] = useRecoilState(hourSelector); // useRecoilState를 selector로 사용
  // useRecoilState를 사용할 때 배열의 첫번째 값은 atom의 값이거나 selector의 get함수의 값
  // 두번째 값은 atom의 값을 수정하는 함수이거나 selector의 set property를 실행하는 함수
  const onMinutesChange = (event: React.FormEvent<HTMLInputElement>) => {
    setMinutes(+event?.currentTarget.value); // minutes의 default는 number인데 input으로 받아온 값은 string이기 때문에 앞에 '+'를 붙임
  };
  const onHoursChange = (event: React.FormEvent<HTMLInputElement>) => {
    setHours(+event.currentTarget.value);
  };
  return (
    <div>
      <input
        value={minutes}
        onChange={onMinutesChange}
        type="number"
        placeholder="Minutes"
      />
      <input
        onChange={onHoursChange}
        value={hours}
        type="number"
        placeholder="Hours"
      />
    </div>
  );
}

export default App;

```
