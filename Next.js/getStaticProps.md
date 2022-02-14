page에서 `getStaticProps` function을 export하면 Next.js가 `getStaticProps` 로부터 return되는 데이터를 가지고 해당 페이지를 build time에 pre-render 한다.

```javascript
export async function getStaticProps(context) {
  return {
    props: {}, // will be passed to the page component as props
  };
}
```

## When should I use getStaticProps?

- 사용자의 요청에 앞서 build time에 페이지를 렌더하는데 필요한 데이터
- headless CMS로부터 오는 데이터
- publicly cached 가능한 데이터
- SEO를 위해 반드시 pre-rendered되어야 하는 데이터 (getStaticProps는 `HTML`과 `JSON` 파일을 생성하고, 둘다 CDN에 의해 캐시될 수 있음)

`getStaticProps` 는 build time에 실행되기 때문에, 들어오는 요청에 대한 access가 없다. 만약 그런 요청에 접근을 해야한다면, `Middleware` 를 사용하는 것을 고려해봐야 한다.

## Using getStaticProps to fetch data from a CMS

```javascript
function Blog({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li>{post.title}</li>
      ))}
    </ul>
  );
}
// This function gets called at build time on server-side.
// It won't be called on client-side, so you can even do
// direct database queries.
export async function getStaticProps() {
  // Call an external API endpoint to get posts.
  // You can use any data fetching library
  const res = await fetch("https://.../posts");
  const posts = await res.json();

  // By returning { props: { posts } }, the Blog component
  // will receive `posts` as a prop at build time
  return {
    props: {
      posts,
    },
  };
}

export default Blog;
```

## Write server-side code directly

`getStaticProps` 는 JS bundle에 포함되지 않는다. 따라서 데이터베이스에 접근하는 쿼리를 브라우저에 보내지 않고도 직접 쓸 수 있다.

따라서 server-side code를 직접 `getStaticProps` 안에 쓰면 된다.

## Statically generates both HTML and JSON

`getStaticProps`를 통해서 페이지가 build time에 pre-rendered될 때, Next.js는 HTML 파일 뿐만 아니라 JSON 파일을 생성한다.

JSON 파일은 `client-side routing`에서 사용된다. (`next/link`, `next/router`) `getStaticProps` 에 의해 pre-rendered된 페이지로 이동할 때, Next.js는 JSON 파일을 fetch하고, 해당 페이지의 컴포넌트를 위한 props로 사용한다.즉 `client-side page transitions` 가 `getStaticProps` 를 호출하는 것이 아니라 오로지 exported된 JSON 파일만이 사용된다는 뜻이다.

## Where can I use getStaticProps

`getStaticProps` 는 오로지 page에서만 exported될 수 있다. 또한 반드시 단독적인 함수로만 export되어야 한다. 다른 함수에 끼어서 export되면 동작하지 않는다.

## Runs on every request in development

`next dev`와 같은 개발 모드에서는 `getStaticProps`가 매 요청마다 호출된다. 따라서 테스트를 위해서는 빌드 모드에서 테스트해야 한다.
