# ServerComponent

> 오직 서버에서만 실행되는 컴포넌트

## 탄생배경

기존의 `CSR`에서는 사용자에게 좋은 인터렉션의 경험을 제공하는 대신 초기로딩속도와 `SEO`라는 치명적인 결함이 존재하였다. 이런 문제를 해결하고자 `SSR방식`을 도입하였는데 초기 로딩속도와 `SEO`라는 문제를 극복할수 있었지만, 서버와 클라이언트 사이의 데이터 동기화 복잡성와, 서버 부하 증가는 새로운 문제로 떠올랐다.

  * [SPA](/BASE/SPA.md)

## 서버 컴포넌트란

> 오직 서버에서만 실행되는 컴포넌트

서버에서 컴포넌트 로직을 실행하고, 필요한 데이터를 패치한 뒤, 최소한의 HTML와 사용자 상호작용에 필수적인 자바스크립트 코드만을 클라이언트로 전송하여 초기 로딩을 개선하고, SEO를 강화하며, 개발 복잡성과 서버 부하 문제를 해결한다.

### 핵심이점

- **클라이언트에서의 Hydration 부재** : 서버에서 완성된 렌더링 결과로 인해 클라이언트에서 `hydration`이 발생하지 않는다.
- **번들크기 감소** : 서버 컴포넌트 로직이 JS번들에 포함되지 않아 번들사이즈가 확연히 줄어든다.

## 사용방법와 활용 

현재 리액트에서는 실험중인 단계로 설정이 매우 복잡하고 많은 의존성이 필요하다  
하지만 `Next.js 13.4`버전부터 `AppRouter`는 기본적으로 모든 컴포넌트가 서버 컴포넌트가 default값이다.

## 클라이언트 컴포넌트 

모든 상황에서 서버 컴포넌트만이 정답일까?? 그렇지 않다. 상태관리나 이펙트가 필요한 경우에는 `use client`를 선언해 클라이언트 컴포넌트로 전환할 수 있다. 이렇게 함으로써 성능상의 이점을 누리면서도 필요에 따라 유연성을 발휘할 수 있다.

## 서버 컴포넌넌트와 SSR은 같은의미?

> 서버 컴포넌트와 서버 사이드 렌더링(SSR)은 모두 웹 애플리케이션의 초기 로딩 성능을 개선하고 SEO를 최적화 하기 위한 기술이다. 그러나 근본적인 실행 방식와 목표에서 차이를 보인다. 

### 서버 컴포넌트의 동작 방식 

서버 컴포넌트에서는 컴포넌트의 로직이 서버에서만 실행되며, 이 과정에서 생성된 HTML은 클라이언트로 전송된다. 여기서 중요한 점은 서버 컴포넌트의 자바스크립트 코드 자체는 클라이언트로 전송되지 않는다는 것이다. 즉, 서버 컴포넌트에 의해 생성된 `HTML`만이 클라이언트로 전송되며, 이 컴포넌트에 필요한 모든 로직 처리는 서버측에서 완료된다. 

### 서버 사이드 렌더링 

SSR에서는 서버가 초기 페이지 로드시 `HTML`을 생성하여 전송하지만, 페이지의 동적인 기능을 지원하기 위해 컴포넌트의 자바스크립트 코드도 함께 클라이언트로 전송되어 하이드레이션을 진행한다.


반면 서버 컴포넌트는 실제 컴포넌트의 코드의 대부분을 서버측에서 처리하고, 클라이언트로는 그 결과물만을 전송함으로써, 전송해야 할 데이터의 양을 최소화 할수 있다.

## 최종 정리

서버 컴포넌트는 단순한 HTML을 클라이언트에게 전달하는 것이 아닌 컴포넌트의 상태와 구조를 나타내는 특별한 형태로 데이터를 전달하여 사용자의 인터렉션에 의해 생성된 클라이언트의 상태를 유지할 수 있을뿐만 아니라 필요에 따라 서버 컴포넌트에서 데이터를 다시 Fetch하고 리렌더링할 때도 클라이언트 상태를 유지할 수 있다.

## 번외 : 하이브리드 렌더링 

> 서버 컴포넌트를 사용하여 `SSR(Server-Side Rendering)`을 구현할 때, 내부적으로 스타일드 컴포넌트를 적용해야 하는 상황에 `‘use client’`를 사용하면서 오류가 발생한다면 하이브리드렌더링 방식으로 서버컴포넌트와 클라이언트 컴포넌트를 조합하여 사용하면 된다

```jsx
import { dehydrate, HydrationBoundary, QueryClient } from '@tanstack/react-query';

import getPostRecommends from '@/app/_lib/getPostRecommends';

import Container from '@/app/(afterLogin)/home/_style/Container';
import Tab from '../_components/tab/Tab';
import PostForm from '../_components/Post/PostForm';
import PostViewList from '../_components/Post/PostViewList';

export default async function Home() {
  const queryClient = new QueryClient();
  await queryClient.prefetchQuery({
    queryKey: ['posts', 'recommends'],
    queryFn: getPostRecommends,
  });

  const dehydratedState = dehydrate(queryClient);

  return (
    <Container>
      <HydrationBoundary state={dehydratedState}>
        <Tab />
        <PostForm />
        <PostViewList />
      </HydrationBoundary>
    </Container>
  );
}

```

하이브리드 렌더링 접근 방식에서는 다음과 같은 단계를 따른다.

1. **서버 사이드 렌더링(SSR)** :  서버에서 `Home` 컴포넌트가 렌더링되어 필요한 데이터를 미리 가져온다. 이 데이터는 서버에서 처리된 후 클라이언트로 전송된다.
2. **클라이언트 사이드 하이드레이션** : 클라이언트는 서버로 부터 받은 데이터와 마크업을 사용하여, 클라이언트 사이드에서 스타일과 동작을 적용한다.
3. **인터랙티브** :  하이드레이션 과정이 완료된 후, 상호작용할 수 있다.

서버 컴포넌트는 데이터를 미리 가져오고 처리하는 데 사용되며, 클라이언트 컴포넌트는 사용자 인터렉션과 관련된 스타일링 및 동작을 담당한다. 서버와 클라이언트 컴포넌트를 조합함으로써, 효율적인 데이터 로딩과 풍부한 사용자 경험을 제공할 수 있다.