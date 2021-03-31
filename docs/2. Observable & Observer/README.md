# Наблюдаемое и наблюдатель

Состояние — это данные, которые управляют приложением. За этим состоянием нужно постоянно наблюдать.

И в случае изменения состояния — как-либо реагировать на эти изменения.

Наблюдатель отслеживает изменения и реагирует на эти изменения (в нашем случае, перерендер)

Начнем с простого счетчика

```js
class Counter extends Component {
  count = 0

  render() {
    return (
      <div className="App">
        <h1>{this.count}</h1>
        <button>-1</button>
        <button>+1</button>
      </div>
    )
  }
}

ReactDOM.render(
  <React.StrictMode>
    <Counter/>
  </React.StrictMode>,
  document.getElementById('root')
)
```

Импортируем нужные нам декораторы `observer` и `observable`
```js
import { observable } from 'mobx'
import { observer } from 'mobx-react'
```

Помечаем декораторами

```js
@observer class Counter extends Component {
  @observable count = 0;
  
  // ...
}
```

Теперь класс `Counter` следит за изменениями счетчика

Затем добавим логику изменения счетчика
```js
@observer class Counter extends Component {
  @observable count = 0;
  
  handleIncrement = () => { this.count++ }
  handleDecrement = () => { this.count-- }
}
```

Чтобы не путаться, что вся логика у нас находится в компоненте, и чтобы убедиться,
что мы используем именно наблюдаемое состояние - вынесем логику наружу компонента

```js
const counterState = observable({
  count: 0,
})

counterState.increment = function() {
  this.count++
}

counterState.decrement = function() {
  this.count--
}

@observer class Counter extends Component {
  handleIncrement = () => { this.props.store.increment() }
  handleDecrement = () => { this.props.store.decrement() }

  render() {
    return (
      <div className="App">
        <h1>{this.props.store.count}</h1>
        <button onClick={this.handleDecrement}>-1</button>
        <button onClick={this.handleIncrement}>+1</button>
      </div>
    )
  }
}

ReactDOM.render(<Counter store={counterState}/>, document.getElementById('root'))
```
