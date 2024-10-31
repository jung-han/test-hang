## PR 추가질문 답

### Medium

> Q. medium.useEventOperations.spec.tsx > 아래 toastFn과 mock과 이 fn은 무엇을 해줄까요?
> Q. medium.integration.spec.tsx > 여기서 ChakraProvider로 묶어주는 동작은 의미있을까요? 있다면 어떤 의미일까요?

- https://vitest.dev/api/vi.html#vi-importactual
- 외부 모듈을 모킹할 때 자주 사용하는 방식입니다.
- 이 테스트는 실제 toast 함수가 제대로 호출이 되었다 -> '그럼 제대로 텍스트가 노출되었을거야' 라는 가정하에 진행되는 테스트 입니다.
- ChakraProvider로 묶인 컴포넌트를 렌더링한다면, 실제 toast 메세지가 노출되었다는 것을 검증할 수 있게 되지만 훅에서는 그런 처리를 하지 않은것이죠.
- https://vitest.dev/api/vi.html#vi-fn
- 이 함수를 통해 어떤 매개변수로 몇 번 호출되었는지 기록하고 그 내용을 검증해 -> '그럼 제대로 텍스트가 노출되었을거야' 라고 검증하는 것입니다.
- 실제로 우리가 리액트 컴포넌트의 루트 지점을 따라가다 보면 여러 provider가 묶여있는 경우들이 있는데요. 이런 부분들은 테스트 설정에 맞춰서 감싸준 뒤 테스트를 진행하는 것이 좋습니다.
  - https://tanstack.com/query/v4/docs/framework/react/guides/testing
  - 많이 사용하는 tanstack query 의 테스트 세팅 부분도 비슷하니 함께 살펴보세요!

> Q. handlersUtils > 아래 여러가지 use 함수는 어떤 역할을 할까요? 어떻게 사용될 수 있을까요?

- 저는 병렬적으로 구동될 수 있는 여러 테스트를 위해, 그리고 실제 구현을 모르는 FE환경에서 테스트를 작성할 수 있도록 독립된 use 환경을 구성했습니다. 이를 통해 테스트 별로 독립적으로 이벤트의 추가 제거를 확인할 수 있고 검증할 수 있게 됩니다. (BE로직에 상관없이요)
- 더 좋은 방법이 있을 수 있는데, 결국 핵심은 독립적이게 구동될 수 있도록 작성하되, 실제 우리가 작성하는 대부분의 통합 테스트 환경에서는 BE의 구현이 중요하지 않다는 지점입니다.
- https://mswjs.io/docs/api/setup-server/use/

> Q. setupTests.ts > 왜 이 시간을 설정해주는 걸까요?

- https://vitest.dev/guide/mocking#mock-the-current-date
- 테스트를 구동하는 날짜와 시간에 영향을 받지 않도록 테스트가 구동되는 시스템 날짜를 정의해줍니다.

### Hard

> Q. handlersUtils에 남긴 질문에 답변해주세요.

- 저는 msw `use`를 사용해 해당 함수를 호출하는 컨텍스트 내에 event 리스트를 유지해 해당 테스트에서만 구동될 수 있는 이벤트 리스트를 만들어서 관리했는데요.
  - https://mswjs.io/docs/api/setup-server/use/
- 결국 핵심은 독립적이게 구동될 수 있도록 작성하되, 실제 우리가 작성하는 대부분의 통합 테스트 환경에서는 BE의 구현이 중요하지 않다는 지점입니다.
  - 응답을 테스트가 필요없을만큼 가볍게 작성하고 유지보수해보세요!

> Q. 테스트를 독립적으로 구동시키기 위해 작성했던 설정들을 소개해주세요.

- MSW 적용
- `useFakeTimers`, `setSystemTime`, `stubEnv('TZ', 'UTC')` 등을 통한 타임존 고정 및 시간 지정
-
