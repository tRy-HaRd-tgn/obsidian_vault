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
# Хуки 
React Redux предоставляет пару хуков для взаимодействия с хран