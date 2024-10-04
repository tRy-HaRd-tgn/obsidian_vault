# Отличия redux от mobx 
Redux позволяет масштабировать приложения, так как он представляет единый интерфейс для управления состоянием, который не зависит от конкретных компонентов. Это облегчает разделение логики и представления, а также повторное использование и композицию компонентов. 
# Установка 
~~~ terminal
npm install react-redux
~~~
# Хранилище
Redux предоставляет компонент Provider
Чтобы к нашему хранилищу был доступ во всем приложении нужно обернуть его в этот компонент
~~~ js
import React from 'react'
import ReactDOM from 'react-dom/client'
import { Provider } from 'react-redux'
import store from './store'
import App from './App'

// React 18
const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(
  <Provider store={store}>
    <App />
  </Provider>
)
~~~
# Принцип работы
Прежде чем как-то изменить состояние мы должны обратиться к диспетчеру и передать action чтобы он выполнил его, но диспетчер не выполняет action, он передает его в reducer, а 
# Хуки 
React Redux предоставляет пару хуков для взаимодействия с хранилищем из ваших компонентов. 
Хук useSelector читает значение из состояния хранилища и подписывается на обновление состояния. И хук useDispatch, возвращающий метод dispatch, для отправки сообщений в хранилище. 
