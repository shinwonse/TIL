![](https://images.velog.io/images/shinwonse/post/508a3154-c053-4dc2-a06a-5e320ec6846f/image.png)
```typescript
function Coins() {
  const [coins, setCoins] = useState<CoinInterface[]>([]);
  const [loading, setLoading] = useState(true);
  useEffect(() => {
    (async () => {
      const response = await fetch("https://api.coinpaprika.com/v1/coins");
      const json = await response.json();
      setCoins(json.slice(0, 100));
      setLoading(false);
    })();
  }, []);
  ...
}
```
다음과 같은 코드를 ```react-query``` 를 사용하여 ```fetch``` 로직을 따로 분리하도록 하겠다.

***src/api.ts***
```typescript
export function fetchCoins() {
  return fetch("https://api.coinpaprika.com/v1/coins").then((response) =>
    response.json()
  );
}
```
이렇게 ```fetch``` 로직을 ```api.ts``` 파일에 따로 분리해둔다. 이 때 ```async await``` 형식이 아닌 오래된 방식인 ```fetch``` 방식을 사용하였다.

***src/routes/Coins.tsx***
```typescript
function Coins() {
  const { isLoading, data } = useQuery<ICoin[]>("allCoins", fetchCoins);
  return (
    <Container>
      <Header>
        <Title>코인</Title>
      </Header>
      {isLoading ? (
        <Loader>Loading...</Loader>
      ) : (
        <CoinsList>
          {data?.slice(0, 100).map((coin) => (
    ...
    }
```
한 눈에 보기에도 엄청나게 간단해진 것을 확인할 수 있다. ```useQuery``` 라는 훅을 사용하였는데 ```useQuery``` 는 총 두가지의 ```argument``` 를 필요로 한다. 첫번째 인자는 ```queryKey``` 이다. 이건 query 의 고유 식별자이다. 두번째 인자는 ```fetcher function``` 이다. 우리는 ```api.ts``` 에 ```fetcher function fetchCoins```를 이미 작성해두었다. 따라서 두번째 인자에 ```fetchCoins``` 를 작성한다.

이번에는 ```useQuery``` 가 받아오는 것을 살펴본다. ```useQuery``` 의 멋진 점 중 하나는 ```isLoading``` 이라고 불리는 ```boolean``` 값을 리턴한다는 점인데 기존에 작성했던 ```const [loading , setLoading] = useState(true)``` 와 같은 ``` useState``` 훅을 대체할 수 있다. ```fetcher function``` 인 ```fetchCoins``` 의 로딩 상태를 ```isLoading``` 이 알려준다는 것이다. 또한 ```fetchCoins``` 가 리턴한 ```json``` 데이터를 ```fetchCoins``` 가 끝나면 그 함수의 데이터를 ```data``` 에 넣어준다.

이뿐만이 아니다. ```react-query``` 는 데이터를 캐시에 저장해둔다. 이 점은 페이지 로딩에서의 이점을 준다.

또 다른 페이지를 ```react-query``` 를 사용하여 바꿔보도록 하겠다.
```typescript
function Coin() {
  const { coinId } = useParams();
  const location = useLocation();
  const priceMatch = useMatch("/:coinId/price");
  const chartMatch = useMatch("/:coinId/chart");
  const state = location.state as RouteState;
  const [loading, setLoading] = useState(true);
  const [info, setInfo] = useState<InfoData>();
  const [priceInfo, setPriceInfo] = useState<PriceData>();
  useEffect(() => {
    (async () => {
      const infoData = await (
        await fetch(`https://api.coinpaprika.com/v1/coins/${coinId}`)
      ).json();
      const priceData = await (
        await fetch(`https://api.coinpaprika.com/v1/tickers/${coinId}`)
      ).json();
      setInfo(infoData);
      setPriceInfo(priceData);
      setLoading(false);
    })();
  }, [coinId]);
```
아까에 비해서 코드량이 많아보이지만 차근차근 바꿔보면
```typescript
function Coin() {
  const { coinId } = useParams();
  const location = useLocation();
  const priceMatch = useMatch("/:coinId/price");
  const chartMatch = useMatch("/:coinId/chart");
  const state = location.state as RouteState;
  const { isLoading: infoLoading, data: infoData } = useQuery<InfoData>(
    ["info", coinId],
    () => fetchCoinInfo(coinId!)
  );
  const { isLoading: tickersLoading, data: tickersData } = useQuery<PriceData>(
    ["tickers", coinId],
    () => fetchCoinTickers(coinId!)
  );
  const loading = infoLoading || tickersLoading;
  return (
    ...
  );
}

```
다음과 같다. ```useQuery``` 는 쿼리 키를 배열 형식으로 받는데 쿼리 키가 중복되지 않도록 ```["info", coinId]``` 같이 쿼리 키를 지정해주었다. 또한 ```useQuery``` 의 ```fetch function``` 인자로 넘길 때는 함수의 형태로 넘겨야 하기 때문에 함수를 바로 실행하는 것이 아닌 ```() => fetchCoinInfo(coinId)``` 로 작성하여 이 함수를 실행하는 함수를 새로 만들어 인자로 넘겨야 함수 자체를 넘길 수 있다.

또한 ```fetch function``` 인 ```fetchCoinInfo``` 에 인자를 넣어줄 때 ```coinId!``` 와 같이 느낌표를 붙였는데 이는데 ```react-router-dom v6``` 버전부터 ```useParams``` 를 사용하게 되면 타입이 string이나 undefined 로 자동 설정되기 때문에 ```!(확장 할당 어선셜)``` 로 값이 무조건 할당되어있다고 컴파일러에게 전달해 값이 없어도 변수를 사용할 수 있게 한다.

![](https://images.velog.io/images/shinwonse/post/6c2c2ecd-c944-4942-8fdd-6d45042ecbd4/%E1%84%92%E1%85%AA%E1%84%86%E1%85%A7%E1%86%AB%20%E1%84%80%E1%85%B5%E1%84%85%E1%85%A9%E1%86%A8%202022-01-20%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.32.44.gif)

위 코드에 대한 결과로 위와 같은 화면이 나오고 ```react-query-devtools``` 를 통해 캐싱도 정상적으로 이루어지고 있다는 것을 알 수 있다.

```react-query``` 라이브러리는 확실히 기존의 데이터 페칭 방법보다 훨씬 효율적이고 또한 매력적인 라이브러리임에 틀림없다.
