# Отличия redux от mobx 
Redux позволяет масштабировать приложения, так как он представляет единый интерфейс для управления состоянием, который не зависит от конкретных компонентов. Это облегчает разделение логики и представления, а также повторное использование и композицию компонентов. 
# Установка 
~~~ terminal
npm install redux react-redux
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
const defaultState = {
	cash: 0,
}
action = {type:'',payload:''}
const reducer = (state,action) =>{
	switch(action.type){
		case:'ADD_CASH'
			return {...state, cash: state.cash + action.payload}
		case:'GET_CASH'
			return {...state, cash: state.cash - action.payload}
		default:
			return state
	}
}
const store = createStore(reducer,)
// React 18
const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(
  <Provider store={store}>
    <App />
  </Provider>
)
~~~
## использование созданного хранилища
~~~ js
const dispatch = useDispatch();
const cash = useSelector(state=>state.cash)
const addCash = (cash) =>{
dispatch({type:'ADD_CASH', payload:cash})
}
const getCash = (cash) =>{
dispatch({type:'GET_CASH', payload:cash})
}
~~~
- cash - состояние которое мы создали
- addCash - функция добавления денег (type - берется из reducer, payload - количество на которые мы увеличиваем величину денег)
# Принцип работы
Прежде чем как-то изменить состояние мы должны обратиться к диспетчеру и передать action чтобы он выполнил его, но диспетчер не выполняет action, он передает его в reducer, а reducer уже выполняет action.
# Хуки 
React Redux предоставляет пару хуков для взаимодействия с хранилищем из ваших компонентов. 
Хук useSelector читает значение из состояния хранилища и подписывается на обновление состояния. И хук useDispatch, возвращающий метод dispatch, для отправки сообщений в хранилище. 
