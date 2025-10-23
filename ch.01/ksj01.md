<!-- PR 제목: Chapter 1. 협력하는 객체들의 공동체 -->

# Chapter 1. 협력하는 객체들의 공동체

## ✏️ 핵심 개념/ 내용 정리

바리스타 - 캐시어 - 커피 주문 고객의 관점에서 비유하는 내용이었고, 각 개념들의 정의를 유추할 수 있었다.

- **객체**: 상태와 행동을 함께 가진 실체. 협력에 참여하는 자율적인 주체
- **메시지**: 객체 간 협력을 위한 유일한 의사소통 수단
- **메서드**: 메시지를 받은 객체가 "어떻게" 처리할지 결정하는 구체적인 방법
- **역할과 책임**: 각 역할은 다양한 책임을 가질 수 있고, 역할들끼리 협력하며 목표를 달성한다.
- **캡슐화**: 내부 구현을 감추고 외부에는 필요한 인터페이스만 제공한다.

## 🧐 깨달은 점

### 1. React 관점에서의 객체지향 개념을 매핑해보았다.

- **객체** : React 컴포넌트 또는 Custom Hook
- **메서드** : state와 action으로 이루어진 핸들러 함수
- **메시지** : Props 또는 Event

### 2. 메시지는 2가지 형태로 볼 수 있다. Props vs Event

- **Props**

```tsx
<ChildComponent onSubmit={handleSubmit} />
```

자식이 `onSubmit()`을 호출하는 것 = 부모에게 메시지 전달한다.

- **Event**

```tsx
onClick={() => onSubmit(data)}
```

사용자 이벤트가 컴포넌트에게 메시지 전달한다.

### 3. 캡슐화와 React의 선언적 방식

책에서 "커피를 만드는 방식은 고객이 고려하지 않아도 된다"는 내용을 읽고 캡슐화 개념이 떠올랐고, React의 선언적 방식과 유사하다고 느꼈다.

**컴포넌트의 캡슐화**

- 다른 개발자가 컴포넌트를 사용할 때 내부 구현을 보지 않아도 된다.
- Props라는 인터페이스만 보고 기능을 이해할 수 있다.
- 잘 추상화된 컴포넌트는 그 자체로 캡슐화다.

이전에 패키지를 뜯어보았던 `useOverlay` 가 떠올랐다.
내부 구현에는 Portal, z-index, 상태관리 코드가 존재했지만, 사용처에서는 어떻게 렌더링이 되고, 어떻게 관리되고, 어떻게 동작하는지는 알 필요 없이, overlay에서 반환하는 메서드만 꺼내어 사용하면 되었다. 이것이 선언적인 코드이자 캡슐화 개념과 동일하다고 느꼈다.

```tsx
const overlay = useOverlay();

overlay.open();
```

## 🖥️ 코드 사례

### 현재 프로젝트의 구조는 UI와 Custom Hook의 협력이다.

UI 컴포넌트와 Custom Hook으로 분리하는 구조 자체가 객체 간 협력이다.

```tsx
function useOrderForm() {
  const [orderList, setOrderList] = useState([]);

  const handleAddOrder = (order) => {
    setOrderList(prev => [...prev, order]);
  };

  return {
    state: { orderList },
    action: { handleAddOrder }
  };
}

function OrderScreen() {
  const { state, action } = useOrderForm();

  return (
    <button onClick={() => action.handleAddOrder({ ... })}>
      주문 추가
    </button>
  );
}
```

- OrderScreen이 useOrderForm을 호출 >> 메시지 전달
- Screen은 Hook 내부 구현을 몰라도 된다 >> 캡슐화

## 🎸 (기타) 논의하고 싶은 주제

### 1. Props vs 이벤트, 어느 것이 더 메시지에 가까운가?

협력의 관점에서 Props 콜백이 더 메시지 개념에 가까워 보인다고 생각한다.
이벤트는 외부 자극이고, Props는 객체 간 직접적인 협력이기 때문..

### 2. 추상화 수준과 캡슐화

useOverlay처럼 잘 추상화된 Hook은 완벽한 캡슐화의 예시다.
그렇다면 "적절한 추상화 수준"은 어떻게 판단할 수 있을까?
