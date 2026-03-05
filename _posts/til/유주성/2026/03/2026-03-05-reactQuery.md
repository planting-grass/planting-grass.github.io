---
title: reactQuery
author: 유주성
date: 2026-03-05
category: TIL/유주성/2026/03
layout: post
---

다른 팀 프론트엔드 개발자와 대화하다가 리액트 쿼리에 대해 이야기를 하게 되었다. 

그게 뭔데..? 나 진짜 기술 잘 모르는구나.. 일단 뭔지 공부를 해보고 쓸지 말지 결정하자. 

공부해 보니, 결국 클라이언트 상태와 백엔드에서 api로 가져온 데이터를 다른 라이브러리를 사용해서 정리하는 것 이였다. 

그럼 이거 쓸지를 생각하고, 클라이언트 사이드 상태를  어떤 것으로 정리해야 할지를 공부해보면 좋을 것 같다. 

## 1. React Query란?

**React Query(TanStack Query)** 는

React 애플리케이션에서 **서버 데이터를 관리하기 위한 라이브러리**이다.

React 애플리케이션에서는 서버 API를 통해 데이터를 가져오는 경우가 많다.

이때 발생하는 **데이터 요청, 캐싱, 로딩 상태, 에러 처리, 데이터 최신화** 등을 효율적으로 관리할 수 있도록 도와준다.

즉, React Query는 **서버 상태(Server State)를 관리하는 라이브러리**이다.

---

# 2. React에서 상태의 종류

React 애플리케이션의 상태는 크게 두 가지로 나눌 수 있다.

### 1) Client State (클라이언트 상태)

브라우저 내부에서만 관리되는 상태

예시

- 모달 열림 여부
- 다크모드
- 입력값
- 필터 선택 상태

보통 다음과 같은 방식으로 관리한다.

- `useState`
- `Zustand`
- `Redux`

---

### 2) Server State (서버 상태)

서버에서 가져오는 데이터

예시

- 사용자 정보
- 게시글 목록
- 매물 데이터
- 통계 데이터

서버 상태의 특징

- 서버에 존재한다
- 비동기 요청이 필요하다
- 여러 컴포넌트에서 공유된다
- 시간이 지나면 데이터가 변경될 수 있다

이러한 특징 때문에 **관리하기가 어렵다.**

---

# 3. React Query가 필요한 이유

React에서 API 데이터를 직접 관리하면 보통 다음과 같은 코드가 작성된다.

```
const [data,setData]=useState(null)
const [loading,setLoading]=useState(true)

useEffect(() => {
fetch("/api/data")
.then(res =>res.json())
.then(result => {
setData(result)
setLoading(false)
    })
}, [])
```

이 방식에는 여러 문제가 존재한다.

### 문제점

1. 로딩 상태를 직접 관리해야 한다
2. 에러 처리를 직접 구현해야 한다
3. 동일한 API를 여러 번 요청할 수 있다
4. 캐싱이 존재하지 않는다
5. 데이터 최신화를 직접 처리해야 한다
6. 컴포넌트마다 코드가 반복된다

React Query는 이러한 문제를 해결하기 위해 등장하였다.

---

# 4. React Query의 주요 역할

React Query는 다음과 같은 기능을 제공한다.

### 1) 데이터 요청 관리

API 호출을 간단한 방식으로 처리할 수 있다.

```
const { data, isLoading, error }=useQuery({
  queryKey: ["posts"],
  queryFn:fetchPosts
})
```

---

### 2) 캐싱 (Caching)

React Query는 요청한 데이터를 **자동으로 캐싱**한다.

예를 들어 여러 컴포넌트에서 동일한 API를 요청하더라도

```
API 요청 → 1번
이후 요청 → 캐시 데이터 사용
```

이를 통해 불필요한 네트워크 요청을 줄일 수 있다.

---

### 3) 자동 데이터 갱신 (Refetch)

React Query는 다음과 같은 상황에서 데이터를 자동으로 다시 가져온다.

- 브라우저 탭으로 다시 돌아왔을 때
- 네트워크가 다시 연결되었을 때
- 설정된 시간이 지났을 때

이를 통해 항상 최신 데이터를 유지할 수 있다.

---

### 4) 데이터 수정 관리 (Mutation)

데이터를 추가, 수정, 삭제하는 작업도 관리할 수 있다.

```
constmutation=useMutation({
  mutationFn:createPost
})
```

데이터 수정 이후에는 다음과 같이 캐시를 갱신할 수 있다.

```
queryClient.invalidateQueries(["posts"])
```

이렇게 하면 해당 데이터를 자동으로 다시 요청한다.

---

### 5) 로딩 및 에러 상태 관리

React Query는 다음과 같은 상태를 자동으로 제공한다.

```
isLoading
isError
error
data
```

이를 통해 UI에서 상태 처리가 쉬워진다.

---

# 5. React Query의 장점

React Query를 사용하면 다음과 같은 장점이 있다.

- 서버 데이터 관리 코드 감소
- 자동 캐싱
- 중복 API 요청 방지
- 로딩 및 에러 상태 자동 관리
- 데이터 최신화 자동 처리

결과적으로 **서버 데이터 관리 로직을 단순화할 수 있다.**

---

# 6. React Query의 역할 정리

React Query는 React 애플리케이션에서 **서버 데이터를 효율적으로 관리하기 위한 라이브러리**이다.

주요 역할

- API 요청 관리
- 데이터 캐싱
- 로딩 상태 관리
- 에러 처리
- 데이터 최신화
- 중복 요청 방지