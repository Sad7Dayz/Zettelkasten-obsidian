## 1. Class Based Components (클래스 기반 컴포넌트)

### 정의

ES6 클래스 문법을 사용하여 React.Component를 상속받아 만드는 컴포넌트입니다. state와 lifecycle 메서드를 사용할 수 있습니다.

### 예시

javascript

```javascript
import React, { Component } from 'react';

class Welcome extends Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <h1>Hello, {this.props.name}</h1>
        <p>Count: {this.state.count}</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          증가
        </button>
      </div>
    );
  }
}

export default Welcome;
```

---

## 2. Functional Components (함수형 컴포넌트)

### 정의

JavaScript 함수로 작성된 컴포넌트입니다. Hooks의 도입으로 state와 lifecycle 기능을 사용할 수 있게 되어 현재 React에서 권장하는 방식입니다.

### 예시

javascript

```javascript
import React from 'react';

function Welcome({ name }) {
  return (
    <div>
      <h1>Hello, {name}</h1>
    </div>
  );
}

// 화살표 함수 방식
const Greeting = ({ message }) => {
  return <p>{message}</p>;
};

export default Welcome;
```

---

## 3. Nested Components (중첩 컴포넌트)

### 정의

컴포넌트 안에 다른 컴포넌트를 포함하여 계층 구조를 만드는 것입니다. 컴포넌트의 재사용성과 코드 구조화를 개선합니다.

### 예시

javascript

```javascript
function UserProfile({ user }) {
  return (
    <div className="profile">
      <Avatar url={user.avatarUrl} />
      <UserInfo user={user} />
    </div>
  );
}

function Avatar({ url }) {
  return <img src={url} alt="avatar" className="avatar" />;
}

function UserInfo({ user }) {
  return (
    <div className="user-info">
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </div>
  );
}
```

---

## 4. List and Keys (리스트와 키)

### 정의

배열 데이터를 반복하여 여러 컴포넌트를 렌더링할 때 사용합니다. `key`는 React가 어떤 항목이 변경, 추가, 삭제되었는지 식별하는 데 도움을 주는 고유한 식별자입니다.

### 예시

javascript

```javascript
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo.id}>
          {todo.text}
        </li>
      ))}
    </ul>
  );
}

// 사용 예시
const todos = [
  { id: 1, text: 'React 공부하기' },
  { id: 2, text: 'Node.js 프로젝트' },
  { id: 3, text: 'MongoDB 연습' }
];

<TodoList todos={todos} />
```

**주의**: 배열 인덱스를 key로 사용하는 것은 항목의 순서가 변경될 수 있는 경우 권장되지 않습니다.

---

## 5. Props (속성)

### 정의

부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달하는 방법입니다. props는 읽기 전용이며 자식 컴포넌트에서 수정할 수 없습니다.

### 예시

javascript

```javascript
// 부모 컴포넌트
function App() {
  return (
    <UserCard 
      name="홍길동" 
      age={25} 
      role="Developer"
      onEdit={() => console.log('편집')}
    />
  );
}

// 자식 컴포넌트
function UserCard({ name, age, role, onEdit }) {
  return (
    <div className="card">
      <h3>{name}</h3>
      <p>나이: {age}</p>
      <p>직책: {role}</p>
      <button onClick={onEdit}>편집</button>
    </div>
  );
}

// children prop 예시
function Container({ children }) {
  return <div className="container">{children}</div>;
}

<Container>
  <h1>제목</h1>
  <p>내용</p>
</Container>
```

---

## 6. Styling React Apps (React 앱 스타일링)

### 정의

React 컴포넌트에 스타일을 적용하는 여러 방법이 있습니다.

### 예시

#### 인라인 스타일

javascript

```javascript
function Button() {
  const buttonStyle = {
    backgroundColor: 'blue',
    color: 'white',
    padding: '10px 20px',
    border: 'none',
    borderRadius: '5px'
  };

  return <button style={buttonStyle}>클릭</button>;
}
```

#### CSS 파일

javascript

```javascript
// Button.css
import './Button.css';

function Button() {
  return <button className="primary-button">클릭</button>;
}
```

#### CSS Modules

javascript

```javascript
// Button.module.css
import styles from './Button.module.css';

function Button() {
  return <button className={styles.primaryButton}>클릭</button>;
}
```

#### Styled Components (라이브러리)

javascript

```javascript
import styled from 'styled-components';

const StyledButton = styled.button`
  background-color: blue;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  
  &:hover {
    background-color: darkblue;
  }
`;

function Button() {
  return <StyledButton>클릭</StyledButton>;
}
```

---

## 7. Conditional Statements (조건문)

### 정의

특정 조건에 따라 다른 UI를 렌더링하는 방법입니다.

### 예시

#### if-else 문

javascript

```javascript
function Greeting({ isLoggedIn }) {
  if (isLoggedIn) {
    return <h1>환영합니다!</h1>;
  } else {
    return <h1>로그인해주세요.</h1>;
  }
}
```

#### 삼항 연산자

javascript

```javascript
function Status({ isActive }) {
  return (
    <div>
      {isActive ? <span>활성</span> : <span>비활성</span>}
    </div>
  );
}
```

#### 논리 AND 연산자

javascript

```javascript
function Notification({ hasMessages, messageCount }) {
  return (
    <div>
      {hasMessages && <span>새 메시지 {messageCount}개</span>}
    </div>
  );
}
```

#### switch 문

javascript

```javascript
function UserRole({ role }) {
  const renderContent = () => {
    switch(role) {
      case 'admin':
        return <AdminPanel />;
      case 'user':
        return <UserPanel />;
      default:
        return <GuestPanel />;
    }
  };

  return <div>{renderContent()}</div>;
}
```

---

## 8. State and setState (상태와 setState)

### 정의

클래스 컴포넌트에서 컴포넌트의 동적 데이터를 관리하는 객체입니다. `setState()`를 통해 상태를 업데이트하면 컴포넌트가 리렌더링됩니다.

### 예시

javascript

```javascript
class Counter extends Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0,
      name: 'Counter'
    };
  }

  increment = () => {
    // 방법 1: 객체 전달
    this.setState({ count: this.state.count + 1 });
  }

  incrementSafe = () => {
    // 방법 2: 함수 전달 (이전 상태 기반 업데이트 시 권장)
    this.setState((prevState) => ({
      count: prevState.count + 1
    }));
  }

  incrementByValue = (value) => {
    this.setState((prevState) => ({
      count: prevState.count + value
    }), () => {
      // setState 완료 후 실행되는 콜백
      console.log('업데이트 완료:', this.state.count);
    });
  }

  render() {
    return (
      <div>
        <h2>{this.state.name}</h2>
        <p>Count: {this.state.count}</p>
        <button onClick={this.increment}>증가</button>
        <button onClick={this.incrementSafe}>안전한 증가</button>
      </div>
    );
  }
}
```

---

## 9. Lifecycle Methods in Class Components (라이프사이클 메서드)

### 정의

클래스 컴포넌트가 생성, 업데이트, 제거되는 과정에서 특정 시점에 실행되는 메서드들입니다.

### 예시

javascript

```javascript
class UserProfile extends Component {
  constructor(props) {
    super(props);
    this.state = {
      user: null,
      loading: true
    };
  }

  // 마운트: 컴포넌트가 DOM에 삽입된 직후
  componentDidMount() {
    console.log('컴포넌트 마운트됨');
    // API 호출, 이벤트 리스너 등록
    this.fetchUser();
  }

  // 업데이트: props나 state가 변경될 때
  componentDidUpdate(prevProps, prevState) {
    console.log('컴포넌트 업데이트됨');
    // 이전 props와 비교하여 조건부 처리
    if (prevProps.userId !== this.props.userId) {
      this.fetchUser();
    }
  }

  // 언마운트: 컴포넌트가 DOM에서 제거되기 직전
  componentWillUnmount() {
    console.log('컴포넌트 언마운트됨');
    // 타이머 정리, 이벤트 리스너 제거 등
    if (this.timer) {
      clearInterval(this.timer);
    }
  }

  // 에러 처리
  componentDidCatch(error, info) {
    console.error('에러 발생:', error, info);
    this.setState({ hasError: true });
  }

  fetchUser = async () => {
    try {
      const response = await fetch(`/api/users/${this.props.userId}`);
      const user = await response.json();
      this.setState({ user, loading: false });
    } catch (error) {
      this.setState({ error, loading: false });
    }
  }

  render() {
    const { user, loading } = this.state;
    
    if (loading) return <div>로딩중...</div>;
    if (!user) return <div>사용자를 찾을 수 없습니다.</div>;
    
    return (
      <div>
        <h2>{user.name}</h2>
        <p>{user.email}</p>
      </div>
    );
  }
}
```

---

## 10. useState Hook

### 정의

함수형 컴포넌트에서 상태를 관리할 수 있게 해주는 Hook입니다. 배열 구조 분해를 통해 현재 상태 값과 상태를 업데이트하는 함수를 반환합니다.

### 예시

javascript

```javascript
import { useState } from 'react';

function FormExample() {
  // 기본 사용법
  const [count, setCount] = useState(0);
  const [name, setName] = useState('');
  const [isActive, setIsActive] = useState(false);
  
  // 객체 상태
  const [user, setUser] = useState({
    name: '',
    email: '',
    age: 0
  });

  // 배열 상태
  const [items, setItems] = useState([]);

  const handleIncrement = () => {
    // 방법 1: 직접 값 설정
    setCount(count + 1);
  };

  const handleIncrementSafe = () => {
    // 방법 2: 함수형 업데이트 (이전 상태 기반)
    setCount(prevCount => prevCount + 1);
  };

  const handleUserUpdate = () => {
    // 객체 상태 업데이트 (스프레드 연산자 사용)
    setUser(prevUser => ({
      ...prevUser,
      name: 'John Doe'
    }));
  };

  const addItem = (newItem) => {
    // 배열 상태 업데이트
    setItems(prevItems => [...prevItems, newItem]);
  };

  // 초기값을 함수로 전달 (비용이 큰 계산일 때 유용)
  const [expensiveValue, setExpensiveValue] = useState(() => {
    return computeExpensiveValue();
  });

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleIncrement}>증가</button>
      <button onClick={handleIncrementSafe}>안전한 증가</button>
      
      <input 
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="이름 입력"
      />
      
      <button onClick={() => setIsActive(!isActive)}>
        {isActive ? '활성' : '비활성'}
      </button>
    </div>
  );
}
```

---

## 11. useEffect Hook and Dependency Logic (useEffect Hook과 의존성 로직)

### 정의

함수형 컴포넌트에서 side effect(부수 효과)를 수행할 수 있게 해주는 Hook입니다. 데이터 fetching, 구독 설정, DOM 수동 조작 등에 사용됩니다.

### 예시

#### 기본 사용법

javascript

```javascript
import { useState, useEffect } from 'react';

function BasicExample() {
  const [count, setCount] = useState(0);

  // 1. 의존성 배열 없음: 매 렌더링마다 실행
  useEffect(() => {
    console.log('매 렌더링마다 실행됨');
  });

  // 2. 빈 의존성 배열: 마운트 시 한 번만 실행
  useEffect(() => {
    console.log('컴포넌트 마운트됨');
    
    // cleanup 함수: 언마운트 시 실행
    return () => {
      console.log('컴포넌트 언마운트됨');
    };
  }, []);

  // 3. 특정 값이 변경될 때만 실행
  useEffect(() => {
    console.log('count가 변경됨:', count);
    document.title = `Count: ${count}`;
  }, [count]);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>증가</button>
    </div>
  );
}
```

#### 여러 의존성 관리

javascript

```javascript
function MultiDependency({ userId, filter }) {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    console.log('userId 또는 filter가 변경됨');
    // userId나 filter 중 하나라도 변경되면 실행
    fetchData();
  }, [userId, filter]);

  const fetchData = async () => {
    setLoading(true);
    // 데이터 가져오기 로직
    setLoading(false);
  };

  return <div>{/* UI */}</div>;
}
```

#### cleanup 함수 활용

javascript

```javascript
function TimerExample() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const timer = setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);

    // cleanup: 타이머 정리
    return () => {
      clearInterval(timer);
      console.log('타이머 정리됨');
    };
  }, []);

  return <div>경과 시간: {seconds}초</div>;
}
```

---

## 12. Data Fetching using useEffect (useEffect를 사용한 데이터 fetching)

### 정의

useEffect Hook을 사용하여 API에서 데이터를 가져오고 컴포넌트 상태를 업데이트하는 패턴입니다.

### 예시

#### 기본 데이터 fetching

javascript

```javascript
import { useState, useEffect } from 'react';

function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchUsers = async () => {
      try {
        setLoading(true);
        const response = await fetch('https://api.example.com/users');
        
        if (!response.ok) {
          throw new Error('데이터를 가져오는데 실패했습니다');
        }
        
        const data = await response.json();
        setUsers(data);
        setError(null);
      } catch (err) {
        setError(err.message);
        setUsers([]);
      } finally {
        setLoading(false);
      }
    };

    fetchUsers();
  }, []); // 빈 배열: 컴포넌트 마운트 시 한 번만 실행

  if (loading) return <div>로딩 중...</div>;
  if (error) return <div>에러: {error}</div>;

  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

#### 동적 매개변수를 사용한 fetching

javascript

```javascript
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    // userId가 없으면 실행하지 않음
    if (!userId) return;

    let isMounted = true; // cleanup을 위한 플래그

    const fetchUser = async () => {
      try {
        setLoading(true);
        const response = await fetch(`/api/users/${userId}`);
        const data = await response.json();
        
        // 컴포넌트가 여전히 마운트되어 있을 때만 상태 업데이트
        if (isMounted) {
          setUser(data);
          setError(null);
        }
      } catch (err) {
        if (isMounted) {
          setError(err.message);
        }
      } finally {
        if (isMounted) {
          setLoading(false);
        }
      }
    };

    fetchUser();

    // cleanup 함수
    return () => {
      isMounted = false;
    };
  }, [userId]); // userId가 변경될 때마다 재실행

  if (loading) return <div>로딩 중...</div>;
  if (error) return <div>에러: {error}</div>;
  if (!user) return <div>사용자를 찾을 수 없습니다</div>;

  return (
    <div>
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </div>
  );
}
```

#### AbortController를 사용한 요청 취소

javascript

```javascript
function SearchUsers({ searchTerm }) {
  const [results, setResults] = useState([]);
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    if (!searchTerm) {
      setResults([]);
      return;
    }

    const controller = new AbortController();
    const signal = controller.signal;

    const searchUsers = async () => {
      try {
        setLoading(true);
        const response = await fetch(
          `/api/users/search?q=${searchTerm}`,
          { signal }
        );
        const data = await response.json();
        setResults(data);
      } catch (err) {
        if (err.name !== 'AbortError') {
          console.error('검색 에러:', err);
        }
      } finally {
        setLoading(false);
      }
    };

    // 디바운싱을 위한 타이머
    const timer = setTimeout(() => {
      searchUsers();
    }, 500);

    // cleanup: 이전 요청 취소 및 타이머 정리
    return () => {
      clearTimeout(timer);
      controller.abort();
    };
  }, [searchTerm]);

  return (
    <div>
      {loading && <div>검색 중...</div>}
      <ul>
        {results.map(user => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

#### 여러 API 호출 조합

javascript

```javascript
function Dashboard() {
  const [data, setData] = useState({
    users: [],
    posts: [],
    comments: []
  });
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchAllData = async () => {
      try {
        setLoading(true);
        
        // 병렬로 여러 API 호출
        const [usersRes, postsRes, commentsRes] = await Promise.all([
          fetch('/api/users'),
          fetch('/api/posts'),
          fetch('/api/comments')
        ]);

        const [users, posts, comments] = await Promise.all([
          usersRes.json(),
          postsRes.json(),
          commentsRes.json()
        ]);

        setData({ users, posts, comments });
        setError(null);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchAllData();
  }, []);

  if (loading) return <div>데이터 로딩 중...</div>;
  if (error) return <div>에러 발생: {error}</div>;

  return (
    <div>
      <h2>사용자: {data.users.length}</h2>
      <h2>게시글: {data.posts.length}</h2>
      <h2>댓글: {data.comments.length}</h2>
    </div>
  );
}
```

#### Custom Hook으로 재사용 가능한 fetching 로직

javascript

```javascript
// useFetch.js - Custom Hook
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    if (!url) return;

    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        const result = await response.json();
        setData(result);
        setError(null);
      } catch (err) {
        setError(err.message);
        setData(null);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return { data, loading, error };
}

// 사용 예시
function App() {
  const { data: users, loading, error } = useFetch('/api/users');

  if (loading) return <div>로딩 중...</div>;
  if (error) return <div>에러: {error}</div>;

  return (
    <ul>
      {users?.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```