<div align="center">
  <h1>Observer Pattern</h1>
</div>

옵저버 패턴은 어떤 객체(subject 또는 observable)에 이벤트가 발생할 때마다 이 객체에 의존하는 다른 모든 객체에 이벤트 발생을 알려주는 디자인 패턴입니다.

## Conceptual example

**observer.ts**

```typescript
interface Observer {
  update(data: unknown): void;
}
```

**subject.ts**

```typescript
interface Subject {
  attach(observer: Observer): void;
  detach(observer: Observer): void;
  notify(observer: Observer): void;
}
```

**concrete-observer.ts**

```typescript
class ConcreteObserver implements Observer {
  update(data: unknown) {}
}
```

**concrete-subject.ts**

```typescript
class ConcreteSubject implements Subject {
  private _observers: Observer[];
  private _state: unknown;

  constructor(initialState: unknown) {
    this._observers = [];
    this._state = initialState;
  }

  attach(observer: Observer) {
    this._observers.push(observer);
  }

  detach(observerToRemove: Observer) {
    this._observers.filter((observer) => observer !== observerToRemove);
  }

  notify(data: unknown) {
    this._observers.forEach((observer) => observer.update(data));
  }

  doSomething() {
    // do something
    // ...
    this._state = newState;
    this.notify(data);
  }
}

const initialState = {};

export const concreteSubject = new ConcreteSubject(initialState);
```

## React example

**observable.ts**

```typescript
type Func = (data: string) => void;

class Observable {
  private _observers: Func[];

  constructor() {
    this._observers = [];
  }

  subscribe(func: Func) {
    this._observers.push(func);
  }

  unsubscribe(func: Func) {
    this._observers = this._observers.filter((observer) => observer !== func);
  }

  notify(data: string) {
    this._observers.forEach((observer) => observer(data));
  }
}

export const observable = new Observable();
```

**componentA.tsx**

```tsx
export const ComponentA = () => {
  const handleClick = () => observable.notify("User clicked button");

  return <button onClick={handleClick}>Button</button>;
};
```

**componentB.tsx**

```tsx
import { useState, useEffect } from "react";

export const ComponentB = () => {
  const [state, setState] = useState("");

  const update = (data: string) => setState(data);

  useEffect(() => {
    observable.subscribe(update);

    return () => {
      observable.unsubscribe(update);
    };
  }, []);

  return <div>{state}</div>;
};
```

## Pros and Cons

## References

- [observer pattern](https://www.patterns.dev/posts/observer-pattern/)
- [observer](https://refactoring.guru/design-patterns/observer)

**[⬅ back to previous](../README.md)**
