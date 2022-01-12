
![](https://images.velog.io/images/shinwonse/post/d295180a-7bb9-41ea-8548-4dd38be1ef60/image.png)
```Static Generation``` 은 데이터를 가져와도 되고 없어도 된다. ```살짝 말이 이상한데...``` 어떤 페이지들은 외부 데이터를 ```fetching``` 해오지 않으면 HTML을 렌더링할 수 없다. 이를 위해 파일 시스템이나 외부 API, 아니면 데이터베이스 등에 ```빌드 타임```에 접근해야 한다. 
![](https://images.velog.io/images/shinwonse/post/61ae64ac-1886-4a8d-97d5-c387a0c31534/image.png)

### Static Generation with Data using `getStaticProps`

Next.js에서는 page 컴포넌트를 export할 때, ```getStaticProps```라는 ```async``` 함수를 export할 수 있다. ```getStaticProps```는 ```build 타임```에 실행되며 함수 안에서 외부 데이터를 ```fetch```하고 페이지에 ```props```로 전달할 수 있다.

```javascript
export default function Home(props) { ... }

export async function getStaticProps() {
  // Get external data from the file system, API, DB, etc.
  const data = ...

  // The value of the `props` key will be
  //  passed to the `Home` component
  return {
    props: ...
  }
}
```
```getStaticProps```는 Next.js에게 '야, 이 페이지는 데이터를 불러와야돼! 너가 이 페이지를 pre-render하기 전에 데이터 먼저 해결해야돼!'라고 전달해준다.

```getStaticProps```를 잘 이해하기 위해서 Next.js 공식 문서의 블로그 예제를 구현할 것이다. 다음과 같은 원리로 말이다.
![](https://images.velog.io/images/shinwonse/post/4637b0ec-7464-4acd-b85d-168e5383dca4/image.png)
```getStaticProps```를 호출하기 전의 준비 코드는 생략하고 바로 ```getStaticProps```를 호출하는 코드를 살펴보겠다.

***pages/index.js***
```javascript
import { getSortedPostsData } from '../lib/posts'

export async function getStaticProps() {
  const allPostsData = getSortedPostsData()
  return {
    props: {
      allPostsData
    }
  }
}

export default function Home({ allPostsData }) {
  return (
    <Layout home>
      {/* Keep the existing code here */}

      {/* Add this <section> tag below the existing <section> tag */}
      <section className={`${utilStyles.headingMd} ${utilStyles.padding1px}`}>
        <h2 className={utilStyles.headingLg}>Blog</h2>
        <ul className={utilStyles.list}>
          {allPostsData.map(({ id, date, title }) => (
            <li className={utilStyles.listItem} key={id}>
              {title}
              <br />
              {id}
              <br />
              {date}
            </li>
          ))}
        </ul>
      </section>
    </Layout>
  )
}
```
위 코드는 파일 시스템 안에 있는 데이터를 ```fetch```해오는 코드이다. 하지만 외부 데이터 API 같은 곳에서도 데이터를 ```fetch```해올 수 있다.
```javascript
export async function getSortedPostsData() {
  // Instead of the file system,
  // fetch post data from an external API endpoint
  const res = await fetch('..')
  return res.json()
}
```
물론 데이터 베이스를 직접 ```query```하는 것도 가능하다.
```javascript
import someDatabaseSDK from 'someDatabaseSDK'

const databaseClient = someDatabaseSDK.createClient(...)

export async function getSortedPostsData() {
  // Instead of the file system,
  // fetch post data from a database
  return databaseClient.query('SELECT posts...')
}
```
```getStaticProps```는 오로지 ```server-side```에서만 동작한다. ```client-side```에서는 동작하지 않는다. ```getStaticProps```는 ```JS bundle```에도 포함되지 않을 것이고 그 말은, 데이터베이스에 직접 접근하는 ```queries```도 브라우저에 보내지지 않으니까 코드에 작성해도 아무런 문제가 없다는 뜻이다.

### Development vs Production
```Development``` 모드에서 ```getStaticProps```는 모든 요청마다 동작한다. 하지만 ```Production``` 모드에서 ```getStaticProps```는 오로지 ```build time```에만 동작한다. 그 말인즉슨, ```request time```에만 접근 가능한 데이터, 예를 들어 ```query parameters나 HTTP headers```를 사용하지 못한 다는 뜻이다.

### Only Allowed in a Page
```getStaticProps```는 오로지 ```page``` 컴포넌트에서만 ```export``` 가능하다. ```non-page``` 파일에서는 불가능하다.

### Fetching Data at Request Time
```request time```에 데이터를 ```fetch```해야 한다면, ```Server-side Rendering```을 고려해볼 필요가 있다.
![](https://images.velog.io/images/shinwonse/post/c5e01f8d-c1d0-40e1-be26-468bc9b061f3/image.png)
다음은 ```getServerSideProps```의 예시 코드이다.
```javascript
export async function getServerSideProps(context) {
  return {
    props: {
      // props for your component
    }
  }
}
```
```request time``` 에 호출되므로, ```context```는 특정한 ```request specific parameters``` 를 포함한다.

아무래도 ```getStaticProps``` 에 비해 ```TTFB(Time to first byte)``` 가 느리므로 꼭 필요할 때만 사용하는 편이 좋다. (``` pre-render a page whose data must be fetched at request time```)

### Client-side Rendering
만약 데이터를 ```pre-render``` 할 필요가 없다면, ```Client-side Rendering``` 을 고려해볼만 하다. 
![](https://images.velog.io/images/shinwonse/post/dbc95cff-d6f6-4999-9797-a7f78d287708/image.png)
