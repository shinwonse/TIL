HTML `<q>` 요소는 둘러싼 텍스트가 짧은 인라인 인용문이라는 것을 나타낸다. 대부분의 브라우저에서는 앞과 뒤에 따옴표를 붙여 표현한다. `<q>` 는 줄 바꿈이 없는 짧은 경우에 적합하며 긴 인용문에는 `<blackquote>` 요소를 사용한다.

```html
<p>
  When Dave asks HAL to open the pod bay door, HAL answers:
  <q cite="https://www.imdb.com/title/tt0062622/quotes/qt0396921"
    >I'm sorry, Dave. I'm afraid I can't do that.</q
  >
</p>
```

위에서 사용한 `cite` 라는 특성은 인용문의 출처 문서나 메시지를 가리키는 URL이며 인용문의 맥락 혹은 출처 정보를 가리킬 용도이다.
