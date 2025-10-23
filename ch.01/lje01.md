# chpater 1. \[협력하는 객체들의 공동체]

## ✏️  핵심 개념/ 내용 정리
### 객체지향의 핵심
- 객체지향의 가장 중요한 개념 세가지 : 역할, 책임, 협력
- **역할?** 어떤 협력에 참여하는 특정한 사람이 협력안에서 차지하는 책임나나 임무
	- 어떠한 집단 안에서 어떠한 임무를 하는 사람
	- 예를들어 카페에서 커피를 주문하는 임무를 맡은 사람은 손님이라는 역할을 가진다
- **책임?** 역할을 맡은 사람이 해야할 임무
	- 예를 들어 선생님은 학생을 가르칠 책임이 있음
- **협력?** 
	- 다른 객체와의 협력 방법 : 메세지 전송(요청과 응답)

##  🧐 깨달은 점?
- react를 객체지향으로 볼 수 있는가?
	- hook이 생기기 이전 (클래스 컴포넌트를 사용했을 때)에는 oop 스타일로 구현되는 경우가 많았음
	- hook이 생긴 이후  공식문서에서도 상속보다는 합성을 더 권장하며 함수형 프로그래밍 패턴을 더 많이 활용하는 편이며 객체지향의 특징 일부를 가지고 있다. 
	- **왜?** react에서 사용되는 JS가 프로토타입 기반 객체지향 패러다임을 따르고있기 때문 [참고](https://turtle-hwan.tistory.com/entry/React%EC%9D%98-Component%EC%99%80-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5%EC%97%90-%EA%B4%80%ED%95%98%EC%97%AC)

- react에서 역할/책임/협력 원칙대로 설계했을 때의 이점/단점은?
	- 높은 응집도와 낮은 결합도
	- 코드 복잡성의 증가 가능성 있음

-  React를 객체지향 관점에 맞게 설계하기 위한 사고방식은?
	- 각 컴포넌트가 어떤 역할(직책)을 맡고, 누구와 어떻게 협력하며, 어떤 책임을 수행할지 먼저 고민
	- (with gpt) UI를 구현하는 순서나 상태 중심이 아니라 “누가, 무엇을, 누구와 함께 하는지”를 먼저 생각하는 사고방식을 할 것
		1. **역할(Role)** → 컴포넌트가 맡는 책임과 의무
	    2. **책임(Responsibility)** → 해당 컴포넌트가 수행해야 할 구체적 행위
	    3. **협력(Collaboration)** → 컴포넌트 간 인터페이스, 데이터/이벤트 전달 방식

- 객체 지향 관점에서  react 코드를 분리하면서 생각해야할 점은?
	- 책의 내용 중 "한 사람이 동시에 여러 역할을 수행할 수 있다" 라는 구문이 있음
	- 각각의 객체(컴포넌트)에게 어디까지 역할을 줄 것인가
		- 한 객체에 너무 많은 책임이 몰리면 유지보수가 어려워지고 결합도가 높아짐
		- 너무 하나의 역할만 하게 되면 객체가 많아져 흐름파악이 어려워지고 가독성이 오히려 저하될 가능성 있음 


## 🖥️ 코드 사례
- **to-be**
```tsx
// 역할: 상태 관리
function useTodoController() {
	const [todos, setTodos] = useState<string[]>([]);
	const addTodo = (title: string) => setTodos([...todos, title]);
	const deleteTodo = (index: number) => setTodos(todos.filter((_, i) => i !== index));
	return { todos, addTodo, deleteTodo };
}

// 역할: 필터링
function useTodoFilterController(todos: Todo[]) {
  const [filter, setFilter] = useState<'all' | 'done' | 'undone'>('all');
  const filteredTodos = todos.filter(t =>
    filter === 'all' ? true : filter === 'done' ? t.done : !t.done
  );
  return { filteredTodos, filter, setFilter };
}

// 역할: UI 표현
function TodoList({ todos, addTodo, deleteTodo }: { todos: string[], addTodo: (t: string) => void, deleteTodo: (i: number) => void }) {
	return (
		<div>
			<ul>
				{todos.map((t, i) => (
					<li key={i}>
						{t} <button onClick={() => deleteTodo(i)}>삭제</button>
					</li>
				))}
			</ul>
			<button onClick={() => addTodo('새 할 일')}>추가</button>
		</div>
	);
}

// 협력 구조: 컨트롤러와 뷰 연결
function TodoPage() {
	const controller = useTodoController();
	return <TodoList todos={controller.todos} addTodo={controller.addTodo} deleteTodo={controller.deleteTodo} />;
}
```

- **설명**
UI를 표시하는 역할(`TodoList`)과 상태를 관리하는 역할(`useTodoController`)을 분리하고,  
두 역할이 props를 통한 명시적 협력(addTodo, deleteTodo 함수 전달)으로 연결된 구조

## 🎸 (기타) 논의하고 싶은 주제

- 각각의 객체(컴포넌트)에게 어디까지 역할을 줄 것인가
	- 한 객체에 너무 많은 책임이 몰리면 유지보수가 어려워지고 결합도가 높아짐
	- 너무 하나의 역할만 하게 되면 객체가 많아져 흐름파악이 어려워지고 가독성이 오히려 저하될 가능성 있음 