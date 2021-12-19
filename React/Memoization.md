# Memoization
메모이제이션은 컴퓨터 프로그램이 **동일한 계산**을 반복해야 할 때, 이전에 계산한 값을 **메모리에 저장**함으로써 동일한 계산의 반복 수행을 제거하여 프로그램 실행 속도를 빠르게 하는 기술이다. (캐시같은 느낌?)

예제 코드를 통해서 이해해보도록 하겠다.

### 예제 코드
##### Memo.jsx
```javascript
import React, { useEffect, useState } from 'react'
import Comments from './Comments'

const commentList = [
    {title:"comment1", content:"message1", likes:1},
    {title:"comment2", content:"message2", likes:2},
    {title:"comment3", content:"message3", likes:3},
]

export default function Memo() {
    const [comments, setComments] = useState(commentList);

    return <Comments commentList={comments} />
}
```
##### Comments.jsx
```javascript
import React from 'react'
import CommentItem from './CommentItem'

export default function Comments({commentList}) {
    return (
        <div>
            {commentList.map(comment => 
            <CommentItem 
                key={comment.title}
                title={comment.title} 
                content={comment.content} 
                likes={comment.likes}
            />)}
        </div>
    );
};
```
##### CommentItem.jsx
```javascript
import React, { memo, Profiler } from 'react'
import './CommentItem.css'

export default CommentItem({title, content, likes}) {
    return (
            <div className='CommentItem'>
                <span>{title}</span>
		<br />
                <span>{content}</span>
		<br />
                <span>{likes}</span>
            </div>
    )
}

```
##### CommentItem.css
```css
.CommentItem {
    border-bottom: 1px solid grey;
    padding: 10px;
    background-color: pink;
    cursor: pointer; 
}
```
이렇게 코드를 준비해뒀다.

## React.memo
**동일한 props**로 렌더링을 한다면, React.memo를 사용하여 **성능 향상**을 누릴 수 있다.

memo를 사용하면 React는 컴포넌트를 렌더링하지 않고 **마지막으로 렌더링된 결과를 재사용**한다.

사용 방법은 다음과 같다.
##### CommentItem.jsx
```javascript
import React, { memo, Profiler } from 'react'
import './CommentItem.css'

function CommentItem({title, content, likes}) {
    return (
            <div className='CommentItem'>
                <span>{title}</span>
                <br />
                <span>{content}</span>
                <br />
                <span>{likes}</span>
            </div>
    )
}

export default memo(CommentItem);
```
그렇다면 위 코드가 최적화가 됐는지 어떻게 알 수 있을까? 예제 코드를 조금 수정하여 props는 같지만 계속해서 리렌더링이 되는 상황을 만들어보겠다.

##### Memo.jsx
```javascript
import React, { useEffect, useState } from 'react'
import Comments from './Comments'

const commentList = [
    {title:"comment1", content:"message1", likes:1},
    {title:"comment2", content:"message2", likes:2},
    {title:"comment3", content:"message3", likes:3},
]

export default function Memo() {
    const [comments, setComments] = useState(commentList)

    useEffect(() => {
        const interval = setInterval(() => {
            setComments((prevComments) => [
                ...prevComments,
                {
                    title:`comment${prevComments.length + 1}`, content:`message${prevComments.length + 1}`, likes:1,
                }
            ])
        }, 1000);

        return () => {
            clearInterval(interval);
        }
    }, []);
    return <Comments commentList={comments} />
}
```
[Profiler
](https://ko.reactjs.org/docs/profiler.html#gatsby-focus-wrapper)
리액트에서는 최적화 툴 Profiler를 제공한다. Profiler는 React 애플리케이션이 렌더링하는 빈도와 렌더링 "비용"을 측정한다. Profiler의 목적은 메모이제이션 같은 성능 최적화 방법을 활용할 수 있는 애플리케이션의 느린 부분들을 식별해내는 것이다.

Profiler를 사용하여 코드를 수정해보겠다.

##### CommentItem.jsx
```javascript
import React, { memo, Profiler } from 'react'
import './CommentItem.css'

function CommentItem({title, content, likes}) {
    function onRenderCallback(
        id,
        phase,
        actualDuration,
        baseDuration,
        startTime,
        commitTime,
        intreactions
    ) {
        console.log(`actualDuration(${title} : ${actualDuration})`);
    }
    return (
        <Profiler id='CommentItem' onRender={onRenderCallback}>
            <div className='CommentItem'>
                <span>{title}</span>
                <br />
                <span>{content}</span>
                <br />
                <span>{likes}</span>
            </div>
        </Profiler>
    )
}

export default memo(CommentItem);
```
여기서 memo를 사용했을 때와 사용하지 않았을 때의 차이점을 비교해보겠다. 우선 memo를 사용하지 않았을 때에는 다음과 같이 아이템들의 렌더링이 처음부터 끝까지 다시 되는 것을 확인할 수 있다.
![](https://images.velog.io/images/shinwonse/post/dd416c34-2aa7-4e0d-97e8-b1757ac78d35/%E1%84%92%E1%85%AA%E1%84%86%E1%85%A7%E1%86%AB%20%E1%84%80%E1%85%B5%E1%84%85%E1%85%A9%E1%86%A8%202021-12-18%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.00.15.gif)
반면 memo를 사용한다면 이는 어떻게 변할까? 다음과 같이 이미 메모이제이션이된 컴포넌트들을 다시 사용하여 새로운 컴포넌트들만 추가하는 것을 볼 수 있다.
![](https://images.velog.io/images/shinwonse/post/4ea84a27-a277-4298-8ea9-34b2dd44957d/%E1%84%92%E1%85%AA%E1%84%86%E1%85%A7%E1%86%AB%20%E1%84%80%E1%85%B5%E1%84%85%E1%85%A9%E1%86%A8%202021-12-18%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.02.18.gif)
## useCallback
그런데 만약 여기서 코드를 이렇게 수정해보면 어떨까? props로 함수를 넘겨주는 것이다.
```javascript
import React from 'react'
import CommentItem from './CommentItem'

export default function Comments({commentList}) {
    return (
        <div>
            {commentList.map(comment => 
            <CommentItem 
                key={comment.title}
                title={comment.title} 
                content={comment.content} 
                likes={comment.likes}
                onClick={() => console.log('눌림')}
            />)}
        </div>
    )
}
```
놀랍게도 이 경우에는 memo를 사용하더라도 사용하지 않은 경우와 마찬가지로 메모이제이션이 사용되지 않고 있는 것을 확인할 수 있다.

메모이제이션을 말할 때 props가 동일한 상태라고 하였다. 하지만 이렇게 inline으로 함수를 준다면 컴포넌트가 렌더링 될 때마다 새로 함수가 만들어지는 것이기 때문에 props가 동일하지 않아 메모이제이션이 일어나지 않는다.

그렇다면 함수를 밖으로 따로 빼보면 어떨까?
```javascript
import React from 'react'
import CommentItem from './CommentItem'

export default function Comments({commentList}) {
    const handleChange = () => {
      console.log('눌림');
    }
    return (
        <div>
            {commentList.map(comment => 
            <CommentItem 
                key={comment.title}
                title={comment.title} 
                content={comment.content} 
                likes={comment.likes}
                onClick={handleChange}
            />)}
        </div>
    )
}
```
결과는 동일하다. 그 이유는 전달받은 commentList가 바뀌기 때문에 Comments도 계속 리렌더링되기 때문이다. 이럴 때 사용하는 것이 바로 ```useCallback```이다.
```javascript
import React from 'react'
import CommentItem from './CommentItem'

export default function Comments({commentList}) {
    const handleChange = useCallback(() => {
      console.log('눌림');
    }, []);
    return (
        <div>
            {commentList.map(comment => 
            <CommentItem 
                key={comment.title}
                title={comment.title} 
                content={comment.content} 
                likes={comment.likes}
                onClick={handleChange}
            />)}
        </div>
    )
}
```
CommentItem에 전달하는 ```handleChange``` 함수가 ```useCallback```을 통해서 메모이제이션되었기 때문에 전달을 받아도 리렌더링이 되지 않는다.

```CommentItem.jsx```에 뭔가를 더 추가해보겠다. 아주 간단하게 rate를 체크하는 코드를.
```javascript
import React, { memo, Profiler, useState } from 'react'
import './CommentItem.css'

function CommentItem({title, content, likes, onClick}) {
    const [clickCount, setClickCount] = useState(0);
    function onRenderCallback(
        id,
        phase,
        actualDuration,
        baseDuration,
        startTime,
        commitTime,
        intreactions
    ) {
        console.log(`actualDuration(${title} : ${actualDuration})`);
    }

    const handleClick = () => {
        onClick();
        setClickCount(prev => prev + 1);
        alert(`${title} 눌림`);
    }
    const rate = () => {
        console.log("rate check");
        return likes > 10 ? 'Good' : 'Bad';
    }

    return (
        <Profiler id='CommentItem' onRender={onRenderCallback}>
            <div className='CommentItem' onClick={handleClick}>
                <span>{title}</span>
                <br />
                <span>{content}</span>
                <br />
                <span>{likes}</span>
                <br />
                <span>{rate()}</span>
                <br />
                <span>{clickCount}</span>
            </div>
        </Profiler>
    )
}
export default memo(CommentItem);
```
그러자 콘솔 창에서 희한한 것을 발견할 수 있었다.
![](https://images.velog.io/images/shinwonse/post/e1821a9a-9b55-4eb4-9052-c2b67ba52d8c/%E1%84%92%E1%85%AA%E1%84%86%E1%85%A7%E1%86%AB%20%E1%84%80%E1%85%B5%E1%84%85%E1%85%A9%E1%86%A8%202021-12-19%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.16.49.gif)
rate를 확인하기 위해 컴포넌트를 클릭하자 해당 컴포넌트가 rate 체크까지 다시하고 있는 것을 볼 수 있다. 상태가 바꼈기 때문에 다시 렌더링되는 것까지는 오케인데 rate 체크까지 다시 하고 있는 것이다. 왜냐면 내가 다시 그려졌기 때문에. 이걸 막기 위한 방법이 바로 ```useMemo```이다. 

```javascript
import React, { memo, Profiler, useMemo, useState } from 'react'
import './CommentItem.css'

function CommentItem({title, content, likes, onClick}) {
    ...
    const rate = useMemo(() => {
        console.log("rate check");
        return likes > 10 ? 'Good' : 'Bad';
    }, [likes]);

    return (
        <Profiler id='CommentItem' onRender={onRenderCallback}>
            <div className='CommentItem' onClick={handleClick}>
                <span>{title}</span>
                <br />
                <span>{content}</span>
                <br />
                <span>{likes}</span>
                <br />
                <span>{rate}</span>
                <br />
                <span>{clickCount}</span>
            </div>
        </Profiler>
    )
}

export default memo(CommentItem);

```

정리해보자면 React.memo는 HOC / Props 비교하여 메모 (컴포넌트 자체를 메모이제이션)

Profiler는 리액트 성능 분석 도구

계산을 통해 만들어진 어떠한 특정한 값을 메모이제이션할때에는 ```useMemo``` 

특정한 함수를 메모이제이션할때에는 ```useCallback```
