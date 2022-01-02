![image](https://user-images.githubusercontent.com/62709718/147244294-3c441e0f-7feb-48d2-b10a-74498cd9519c.png)

---

## React Query란?
react-query는 React Application에서 서버 상태를 가져오고, 캐싱하고, 동기화하고, 업데이트하는 작업을 함

## Motivation
1. 기본적으로 React Application에서는 서버로부터 데이터를 가져오거나 업데이트하는 방법을 제공하지 않음

2. 대부분의 전통적인 상태 관리 라이브러리는 비동기처리나 서버상태 작업보다는 클라이언트 상태 관리에 적합
    > 여기서 서버상태란?
    > - 사용자가 제어하지 못하는 위치에서 원격으로 유지
    > - 비동기 API를 통해 서버 상태를 가져오거나 업데이트
    > - 소유권이 다른 사람과 공유
    > - 주의하지 않으면 Application에서 잠재적으로 out-of-date 상태가 될 수 있음

3. 클라이언트에서 서버 상태를 처리할 때 다양한 문제를 마주함
    > - 캐싱
    > - 백그라운드에서 out-of-date된 데이터를 갱신해야 할 때
    > - 데이터가 out-of-date될 때를 파악해야 할 때
    > - 가능한 빠르게 데이터 업데이트를 반영해야 할 때
    > - 페이징이나 지연 로딩 데이터를 최적화해야 할 때
    > - 메모리 가비지 컬렉션을 운영해야 할 때
    > - 쿼리 결과를 메모이제이션

## Important Defaults
```useQuery```, ```useInfiniteQuery```를 통한 쿼리 인스턴스는 기본적으로 캐시도니 데이터를 오래된(stale) 데이터로 간주함

오래된 쿼리는 아래와 같은 경우 백그라운드에서 자동으로 다시 가져옴