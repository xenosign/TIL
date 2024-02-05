# useQuery 사용하기

## 기본 사용법

- useQuery 는 컴포넌트 최초 로딩 시, 데이터를 Fetch 할 때 사용
- queryKey 와 queryFn 을 사용하여 데이터 요청을 보낼 수 있다
- queryKey 는 ReactQuery 가 캐싱을 관리를 하는 기준이 된다
- queryFn 은 요청을 보내는 함수, 즉 기존의 fetch 내용을 담으면 된다

```jsx
async function getPosts() {
  const response = await fetch(`${BASE_URL}/posts`);
  return await response.json();
}

function HomePage() {
  const { data: postData } = useQuery({
    queryKey: ["posts"],
    queryFn: getPosts,
  });

  const posts = postData?.results ?? [];

  console.log(posts);

  return (
    <div>
      <h1>홈페이지</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>
            {post.user.name}: {post.content}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default HomePage;
```

## staleTime & gcTime

- staleTime

  - 쿼리의 최신 상태를 갱신하는 시간 (기본 0초)
  - 기본 값은 0 이기 때문에 새로 요청이 들어오면 백엔드에 다시 요청을 보내서 결과를 받는다
  - 밀리 세컨드 단위로 시간을 정해주면, 해당 시간 동안 다시 요청이 필요할 경우 캐싱 된 데이터를 가져온다 (통신 X)

- gcTime
  - 쿼리를 요청한 컴포넌트가 언마운트 되었을 때, 캐시에 저장 된 데이터를 삭제하는 시간 (기본 5분)

```jsx
const { data: postData } = useQuery({
  queryKey: ["posts"],
  queryFn: getPosts,
  staleTime: 60 * 1000,
  gcTime: 60 * 1000 * 10,
});
```

![useQuery 라이프사이클](./image/useQuery_life_cycle.png);
