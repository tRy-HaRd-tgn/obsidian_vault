 # Local Storage
Веб хранилище находящееся на клиенте пользователя, позволяет сохранять данные размером до 10 мб.
Хранение осуществляется в виде  ключ - значение
~~~ js
localStorage.removeItem('auth');
~~~
- Пример удаления значения 
~~~ js
localStorage.setItem('auth','true')
~~~
- Пример заполнения значений
## Асинхронные функции
Асинхронные функции позволяют производить выполнение функции параллельно с загрузкой веб-страницы.
# Циклы внутри компонента
В javascript нельзя использовать циклы (for, while, do while)
Нужно использовать функцию map
~~~ javascript
const arr = [1,2,3,4]
{
arr.map((value,index)=>{
	return <h2 key={index}>{value}</h2>
})
}
~~~
- value - значение из массива
- index - итератор который равен нынешнему значению из массива
- key задавать ОБЯЗАТЕЛЬНО!
- return писать  ОБЯЗАТЕЛЬНО
# метод  shift
метод массива при использовании удаляющий первый элемент массива (возвращает удаленный элемент). Метод меняет длину массива!
# метод promt
метод позволяющий вывести диалоговое окно, запрашивающее на ввод данные