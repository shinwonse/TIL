HTML `<span>` 요소는 구문 콘텐츠를 위한 통용 인라인 컨테이너로, _본질적으로는 아무것도 나타내지 않는다._ 스타일을 적용하기 위해서, 또는 `lang` 등 어떤 특성의 값을 서로 공유하는 요소를 묶을 때 사용할 수 있다.

_적절한 의미를 가진 다른 요소가 없을 때에만 사용해야 한다._ `<span>` 은 인라인 요소이고, `<div>` 는 블록 레벨 요소이다.

```html
<p>
  <span>some text</span>
</p>
```

```html
<li>
  <span>
    <a href="portfolio.html">See my portfolio</a>
  </span>
</li>
>
```
