![image](https://user-images.githubusercontent.com/62709718/148638192-88f71f1d-9359-4684-9e0d-cdd66dcde20f.png)

## Next.js란?

Vercel이 만든 React ***Framework***. 라이브러리를 표방한 React의 장점은 살리되, 다양한 편의 기능들을 추가한 Framework의 필요를 충족시킴

## Next.js하면 항상 나오는 단어 SSR과 CSR
### SSR
클라이언트로 전달될 HTML 파일 내용 (일부를) 미리 그려서 내려주는 것
- 클라이언트 렌더링의 속도를 빠르게 하여, 사용자 체감 속도 증진
- 검색 엔진이 JavaScript를 실행하지 않고 크롤링 가능 SEO
<img width="591" alt="스크린샷 2022-01-08 오후 6 14 22" src="https://user-images.githubusercontent.com/62709718/148638817-455f1e60-b84e-4d35-a14e-85a01ef4f349.png">
이처럼 자바스크립트를 끄더라도 화면에 일부가 그려짐

### CSR
자바스크립트를 사용자의 환경에서 다운로드받아야만 화면이 보이고 실행이 가능함

## 각종 주요 기능
- 각종 Optimization
- Hot Code Reloading
- Automatic Routing
- Automatic Code Splitting
- Prefetching
- Dynamic Components Import
- Static Exorts
