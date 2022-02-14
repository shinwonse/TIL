`getServerSideProps` function을 page에서 export하면 Next.js가 해당 페이지를 request time마다 pre-render 한다. 이때 `getServerSideProps` 로부터 return 받은 데이터를 사용한다.

`getServerSideProps` 는 변화가 자주 일어나는 데이터를 fetch 하거나, 가장 최근 데이터로 업데이트를 시켜야하는 page에서 유용하다.

`getServerSideProps` 에서 쓰기 위한 모듈들을 top-level-scope에서 import 할 수 있다. `server-side code` 들을 직접적으로 `getServerSideProps` 안에 작성할 수 있다. 데이터베이스로부터 데이터를 fetching하는 작업 또한 마찬가지이다.

단 TTFB(Time to First Byte)는 `getStaticProps` 보다 나빠질 수 있다.

```javascript
export async function getServerSideProps(context) {
  return {
    props: {}, // will be passed to the page component as props
  };
}
```

`getServerSideProps` 는 browser 에서는 절대 동작하지 않고 오로지 server-side 에서만 동작한다.

- `getServerSideProps` 를 호출한 page를 직접 요청시 `getServerSideProps` 는 request time에 동작하며, page는 returned props와 함께 pre-rendered된다.
- `next/link` 나 `next/router` 를 통해 해당 page를 요청하면, Next.js는 서버에 `getServerSideProps`를 동작시키는 API request를 보낸다.

그리고 해당 페이지를 렌더링할때 쓰이는 JSON 데이터를 `getServerSideProps`가 결과값으로 내놓는다.

`getServerSideProps`는 오로지 page로부터만 export될 수 있다. 다른 파일에서는 안된다. 또한 `getServerSideProps`는 독자적으로 쓰여야 한다. page component의 property로 쓰이거나 하면 안된다.

## Fetching data on the client side

계속 업데이트되는 데이터가 있는 페이지라면 굳이 데이터를 pre-render할 필요가 없다. 대신 `client-side` 로부터 데이터를 fetch하면 된다.

`user-specific` 한 데이터를 예시로 들어보면,

- 먼저, 데이터 없이 즉시 페이지를 보여준다. 페이지의 일부는 `Static Generation` 을 통해 pre-rendered 될 수 있다. 그리고 비어있는 데이터는 로딩중임을 표시해주면 된다.
- 그리고 `client side` 에서 데이터를 fetch 한 후 준비되면 보여주면 된다.

이런 방법은 private하고 user-specific 한 페이지에서 사용할 수 있을 것이다. 이러한 페이지는 SEO도 딱히 필요없고, 그냥 요청 시에 데이터 페칭만 하면 된다.

## Using getServerSideProps to fetch data at request time

```javascript
function Page({ data }) {
  // Render data...
}

// This gets called on every request
export async function getServerSideProps() {
  // Fetch data from external API
  const res = await fetch(`https://.../data`);
  const data = await res.json();

  // Pass data to the page via props
  return { props: { data } };
}

export default Page;
```
