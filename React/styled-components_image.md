![](https://images.velog.io/images/shinwonse/post/83fdcd4e-8acd-4c59-8967-a284f18c9697/image.png)

styled-components를 사용할 때 이미지 컴포넌트를 직접 ```styled.img```로 만들것인지 ```styled.div```안에 background-image로 이미지를 넣을 것인지 어느게 나은 방법인지 잘 모르겠다. 그래서 이번 기회에 다른 개발자들은 어떻게 사용하고 있는지 알아보기로 하였다.

> 참고
https://stackoverflow.com/questions/41346413/react-js-styled-components-importing-images-and-using-them-as-div-background

## Using image as background-image CSS property

```javascript
import LogoSrc from './assets/logo.png';

/* ... */

const LogoDiv = styled.div`
  background-image: url(${LogoSrc});
  /* width and height should be set otherwise container will have either have them as 0 or grow depending on its contents */
`;

/* ... */

<LogoDiv />
```

## Normal way of importing and using images 
```javascript
import LogoSrc from './assets/logo.png';

/* ... */

const Logo = styled.img`
    width: 30px;
    height: 30px;
    margin: 15px;
`;

/* ... inside the render or return of your component ... */

<Logo src={LogoSrc} />
```

## When using components that you already import (i.e. ant-design components of from other component library)

Logo 파일을 export하고 다른 컴포넌트에서 import하는 상황이라고 가정하면

```javascript
const Logo = styled.img`
    width: 30px;
    height: 30px;
    margin: 15px;
`;

export default Logo;
```
어디에서 import하든지 style을 아래와 같이 또 추가할 수 있다.
```javascript
import Logo from '../components/Logo';

const L = styled(Logo)`
   border: 1px dashed black;
`;

/* ... then inside render or return ... */
<L />
```

static한 이미지 파일에서는 아래와 같은 방법도 좋아보인다
```javascript
import logo from 'public/images/logo.jpg';

/* ... */

const HeaderImg = styled.img.attrs({
  src: `${logo}`
})`
width: 50px;
height: 30px;
`;
```
