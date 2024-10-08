# Концепция react
У нас есть один главный hml файл в нем есть теги body, head или какие-либо мета теги при этом все содержимое, все данные приходят из javascript (то есть как таковой html разметки мы не пишем).
# Принцип действия react и его плюсы
Управлять всем содержимым мы можем абсолютно как хотим() что-то удалять, что-то добавлять) при изменении, то есть мы получаем достаточно быстрое и отзывчивое приложение (на первичную загрузку мы тратим больше времени зато потом приложение работает гораздо быстрее и пользователю комфортнее с ним работать (первичную загрузку также можно сделать быстрее если грамотно использовать webpack и различные другие хитрости)).
# Компонентный подход react 
React работает по принципу компонентного подхода. Мы создаем какое-то количество компонентном и используем их как кирпичики, вкладывая компоненты друг в друга мы создаем довольно таки сложные интерфейсы, которые мы потом можем переиспользовать. При таком подходе всегда есть какой-то корневой элемент который вмонтируется в index.html файл, такой компонент в контексте react называется app.
![[react tree.png]]
Для своей оптимизации react строит Virual DOM - так называемая легковесная копия DOM - дерева, когда в каких-то узлах этого дерева произошли какие-то изменения react не переносит эти изменения в DOM - дерево сразу а составляет новое дерево элементов с обновленными значениями и сравнивает с предыдущим - эта стадия называется согласованием. После того как react нашел отличия в деревьях происходит фаза визуализации(рендеринга), причем присутствует механизм приоритетов( это нужно для того, чтобы при визуализации происходила плавно )
![[согласование.png]]
# Создание первого react проекта и его запуск
1: Можно воспользоваться утилитой create-react-app, команда выглядит так 
~~~
npm create-react-app project-name
~~~
2: Запуск такого проекта производиться с помощью команды npm start
# Комбинации клавиш
alt + i - очистка неиспользуемых импортов
# Что такое jsx
Jsx - расширение языка js для упрощения написания пользовательского интерфейса
# Комментирование в react
~~~ js
{/* …*/}
~~~
# Вывод значений из переменных 
~~~ js
<p>{likes}</p>
~~~
- где likes - переменная, а { } нужны для того чтобы писать js код
# Предотвращаем распространение event на дочерние элементы
~~~ js
onClick={(e) =>{e.stopPropagation()}}
~~~
# Стилизация элементов DOM - дерева
## 1 способ classname
~~~ js
<p classname="" >something</p>
~~~
- пример использования
## 2 способ inline стили
~~~ js
<h1 style={{ textAlign: "center" }}>Список постов</h1>
~~~
- пример использования
## 3 способ задание стилей модульно
1: создаем css файл 
![[создание css модуля.png]]
2: применяем стили
~~~ js
import classes from  './MyButton.module.css'

function MyButton(props) {

    return (

        <button className={classes.myBtn}>

        </button>
    )
}
~~~
- где myBtn класс который мы хотим применить
# React хуки
Мы можем использовать хуки только на высшем уровне вложенности ( объявление и использование хуков внутри функций, циклов, условиях запрещено) 
## Кастомные хуки
Выглядят как функции, внутри которых используются стандартные react хуки
## Пример с сохранением отсортированного массива
~~~ js
import { useMemo } from "react";

export const useSortedPosts = (posts, sort) => {

  const sortedPosts = useMemo(() => {

    if (sort) {

      return [...posts].sort((a, b) => a[sort].localeCompare(b[sort]));

    }

    return posts;

  }, [sort, posts]);

  return sortedPosts;

};

  

export const usePosts = (posts, sort, query) => {

  const sortedPosts = useSortedPosts(posts, sort);

  const sortedAndSearchedPosts = useMemo(() => {

    return sortedPosts.filter((post) =>

      post.title.toLocaleLowerCase().includes(query)

    );

  }, [query, sortedPosts]);

  return sortedAndSearchedPosts;

};
~~~
- Пример использования 
## Пример с сохранением данных о загрузке данных
~~~ js
import { useState } from "react";

import React  from "react";

export const useFetching = (callback) => {

  const [isLoading, setIsLoading] = useState(false);

  const [error, setError] = useState("");

  const fetching = async () => {

    try {

      setIsLoading(true);

      await callback();

    } catch (e) {

      setError(e.message);

    } finally {

      setIsLoading(false)

    }

  };

  return [fetching, isLoading, error]

};
~~~
- где fetching - асинхронный метод 
- isLoading - булевое значение дающее понять в процессе ли выполнения метод
- error - позволяет определить есть ли ошибка и если она есть то какая
~~~ js
const [fetchPosts, isPostsLoading, postError] = useFetching(async () => {

    const posts = await PostService.getAll();

    setPosts(posts);

  });
~~~
- Объявление хука
~~~ js
useEffect(() => {

    fetchPosts();

  }, []);
~~~
- Пример использования (заполнение массива posts данными с сервера при загрузке страницы)
## useState
Позволяет работать с состоянием объектов. Пример использования - у нас есть счетчик и две кнопки увеличивающая и уменьшающая значение. Чтобы динамически изменять значение и выводить его пользователю нам и нужно воспользоваться useState.
### Синтаксис объявления состояния 
~~~ js
const [count,setCount] = useState(0);
~~~
Наращивание количества лайков 
~~~ js
setCount(count+1);
~~~
### пример связывания input с h3 с помощью useState
Передавая как аргумент функции onChange event мы можем получить данные элемента который вызывает данную функцию, в данном случае это позволяет нам достать из input его value.
~~~ js
      <input

        onChange={(event) => {

          setValue(event.target.value);

        }}

      ></input>

      <h3>{value}</h3>
~~~
### Добавление в состояние с массивом объектов
~~~ js
setPosts([...posts, newPost]);
~~~
- где posts - массив, а newPost - новый объект 
### Состояние с несколькими полями
~~~ js
const [post, setPost] = useState({ title: "", body: "" });
setPosts([...posts, { ...post, id: Date.now() }]);
~~~
### Работа с input в react(состояние)
~~~ js
<MyInput

        value={title}

        onChange={(e) => {

          setTitle(e.target.value);

        }}

        type="text"

        placeholder="Название поста"

      ></MyInput>
~~~
- данный код используется для того чтобы сохранять в состоянии данные из input
## Сортировка данных по именам или описаниям
~~~ js
  const sortPost = (sort) => {

    setSelectedSort(sort);

    setPosts([...posts].sort((a,b)=> a[sort].localeCompare(b[sort])))

  };
~~~
- где сорт это название параметров по которым по хотим отсортировать данные
## useRef
С помощью этого хука мы можем получить доступ к DOM элементу.
Условно говоря это продвинутая версия querySelector
### Объявление хука
~~~ js
const bodyInputRef = useRef();
~~~
### Пример связывания хука и элемента DOM 
~~~ js
ref={bodyInputRef}
~~~
- Данная строчка прописывается внутри тега, после этого по bodyInputRef можно доставать информацию элемента
### Использование useref в связке с компонентам
Передаем ref{название useRef}
~~~ js
<MyInput

        ref={bodyInputRef}

        type="text"

        placeholder="Описание поста"

></MyInput>
~~~
- Нужно использовать функцию React.forwardRef(), в который передаем ref
~~~ js
const MyInput = React.forwardRef((props, ref) => {

  return <input ref={ref} {...props} className={classes.myInput} />;

});
~~~
это позволит нам доставать поля из input написанного в компоненте
Данный компонент называется неуправляемым
## useMemo
Данный хук производит вычисления запоминает результаты и кеширует, подобное поведение называется мемоизация и на каждую отрисовку элемента она не пересчитывается 
### Пример использования
~~~ js
const sortedPosts = useMemo(() => {

    console.log("отработала функция сортировки");

    if (selectedSort) {

      return [...posts].sort((a, b) =>

        a[selectedSort].localeCompare(b[selectedSort])

      );

    }

    return posts;

  }, [selectedSort, posts]);
~~~
- callback отрабатывает только в том случае, когда изменяются selectedSort или post
## useCallback
## useContext
хук который позволяет создать глобальное хранилище с данными
### Импортирование
~~~ js
import { createContext } from "react";
export const AuthContext = createContext(null)
~~~
- Создание контекста
### Использование
Чтобы воспользоваться контекстом нужно обернуть приложение в созданный контекст
~~~ js
import { BrowserRouter } from "react-router-dom";
import Navbar from "./UI/navbar/Navbar";
import AppRouter from "./components/AppRouter";
import { AuthContext } from "./context";
import { useState } from "react";
function App() {

  const [isAuth, setIsAuth] = useState(false);

  return (

    <AuthContext.Provider value={{

      isAuth,

      setIsAuth: setIsAuth()

    }}>

      <BrowserRouter>

        <Navbar></Navbar>

        <AppRouter></AppRouter>

      </BrowserRouter>

    </AuthContext.Provider>

  );

}

export default App;
~~~
- Создаем состояние и аргументами передаем в наш контекст поле и функцию изменения состояния
## useEffect
### Предназначение
Предназначен для слежки за изменением данных 
### Принцип работы  
Работает асинхронно, связано это с тем, что данный хук часто бывает тяжеловесным и отрисовка измененных состояний происходит даже в том случае если useEffect ещё не отработал до конца.
### Синтаксис
~~~ js
useEffect(callback,deps)
useEffect(()=>{
	fetchPosts()
	return(){
	}
},[])
~~~
- [] - отработает 1 раз в момент монтирования компонента
- [filter] - будет отрабатывать каждый раз при изменении filter
- return функция будет вызвана в момент размонтирования
### Пример использования
Может быть использован к примеру для создания таймера, то есть для работы со значением состояния, а не создавать таймер при каждой отрисовке компонента.
При использовании хука для таймера можно использовать конструкцию для его очистки
~~~ js
return () => {clearInterval(interval)}
~~~
- возвращаемая функция запускается перед следующим вызовом useEffect
### Пример использования
~~~ js
async function fetchPosts() {

    const response = await axios.get(

      "https://jsonplaceholder.typicode.com/posts"

    );

    console.log(response.data);

    setPosts(response.data)

  }

  useEffect(()=>{

    fetchPosts()

  },[])
~~~
- С помощью useEffect проверяем нужно ли что-то подгружать
### Пример использования при размонтировании компонента
В том случае когда через useEffect мы подгружаем данные с сервера по нажатию какой-либо кнопки.
~~~ js
const abortController = new abortController();
return () =>{
	abortController.abort()
}
~~~
- Можно прервать запрос на сервер с учетом того, что пользователь опять нажал на кнопку, а данные ещё не успели отобразиться. Это делается для оптимизации работы.
## useNavigate
Хук предназначен для осуществления динамической навигации
### импортирование 
~~~ js
import { useNavigate } from "react-router-dom"
~~~
### Пример использования
~~~ js
const PostItem = (props) => {

  const router = useNavigate();

  let path ='/posts/' + props.post.id;

  return (

    <div className="post">

      <div className="post_content">

        <strong>

          {props.post.id}. {props.post.title}

        </strong>

        <div>{props.post.body}</div>

      </div>

      <div className="post_btns">

        <MyButton onClick={() => router(path)}>

          Открыть

        </MyButton>

        <MyButton onClick={() => props.remove(props.post)}>Удалить</MyButton>

      </div>

    </div>

  );

};
~~~
- Изменяем адресную строку ну posts/'id'

суть работы заключается в объекте истории содержащем некоторые методы, которые мы можем использовать для навигации между маршрутами внутри компонента
есть свойство location - показывающее текущий путь
есть функции go и goBack, push(можем переходить на конкретную страницу без использования ссылок)
~~~ js
<MyButton onClick={() => router.push('/posts/&{props.post.id}')}>Открыть</MyButton>
~~~
- пример использования - позволяет перейти на страницу поста по id
~~~ js
<Route path="/posts/:id" element={<PostIdPage></PostIdPage>}></Route>
~~~
- Чтобы сделать путь динамическим нужно написать /:id
# useParams
хук предназначенный для изменения или получения доступа к отдельным составляющим URL
# Компоненты
## Жизненный цикл компонента
Происходит в три этапа
За изменением состояния может следить хук useEffect
### Монтирование (mount)
Создание компонента и вмонтирование его в DOM дерево
### Обновление (update)
К примеру при изменение состояние, происходит обновление компонента
### Размонтирование (unmount)
Когда по какой-либо причине компонент нам больше не нужен и мы его удаляем
## Пример объявления компонента
Внутри return мы должны прописать html код
~~~ js
const Counter = () => {

  return <body></body>;

};
export default Counter;
~~~
## Добавление компонента
Для добавления компонента в конце компонента должно быть прописано export default и название корневого элемента- это делает для того чтобы react мог подхватить этот компонент и вывести его
~~~ js
import Count from './components/Counter'
<Counter/>
~~~
# Обмен данными между компонентами
props - некоторые аргументы, параметры , которые может принимать компонент из вне, но обмен этими данными идет только сверху вниз
### Callback
происходит путем передачи в компонент функции, которая что-либо выполняет с данными из компонента в родительском элементе
~~~ js
<PostForm create={createNewPost}></PostForm>

const createNewPost= (newPost)=>{

    setPosts([...posts,newPost])

  }
~~~
- где createNewPost - функции которая должна вызываться из дочернего компонента
- createNewPost выполняет изменения в состоянии в котором находятся объекты, которые рисует другой компонент
~~~ js
const addNewPost = (e) => {

    e.preventDefault();

    const newPost = {

      ...post,

      id: Date.now(),

    };

    create(newPost);

    setPost({ title: "", body: "" });

  };
~~~
- Данная функция по нажатию на кнопку(в дочернем компоненте) создает объект содержащий в себе информацию с input и вызывает переданную родителем функцию добавления объекта в массив, который отвечает за вывод все объектов на экран
## Props React
Компонент может принимать дополнительные данные, эти данные называются props
~~~ cpp
import React from "react";

const PostItem = (props) => {
  return ();
};

export default PostItem;
~~~
### props.children
специальный пропс отвечающий за информацию которую вы написали внутри тега
~~~ js
function MyButton({ children, ...props }) {

  return <button {...props} className={classes.myBtn}>{children}</button>;

}
~~~

~~~ js
<MyButton type="submit" onClick={addNewPost}>
        Создать пост
</MyButton>
~~~
- внутрь компонента MyButton попадает информация "Создать пост"  в виде поля children объекта props
### Передача информации в компонент 
~~~ cpp
<PostItem post={{id:1,title: 'Javascript', body: 'Description'}}></PostItem>
~~~
### Пример использования props
~~~ cpp
import React from "react";

const PostItem = (props) => {

  return (

    <div className="post">

      <div className="post_content">

        <strong>

          {props.post.id}. {props.post.title}

        </strong>

        <div>{props.post.body}</div>

      </div>

      <div className="post_btns">

        <button>Удалить</button>

      </div>

    </div>

  );

};

export default PostItem;
~~~
### Пример отрисовки компонентов с передачей в них массива объектов
При использовании такого подхода react все будет отрисовывать, но будет просить указание key, это нужно для того чтобы react мог понимать какой именно из указанных компонентов нужно перерисовать, при указании key нужно использовать уникальный идентификатор, использование индекса массива в этом случае некорректна , потому что при удалении  объектов это может вызвать дополнительные трудности
~~~ cpp
function App() {

  const [posts, setPosts] = useState([

    { id: 1, title: "Javascript", body: "Description" },

    { id: 2, title: "Javascript 2", body: "Description" },

    { id: 3, title: "Javascript 3", body: "Description" },

  ]);

  return (

    <div className="App">

      {posts.map((post) => (

        <PostItem post={post} key={post.id}></PostItem>

      ))}

    </div>

  );

}
~~~
### Пример с отрисовкой компонента с абсолютно всеми переданными свойствами
~~~ cpp
function MyButton({ children, ...props }) {

  return <button {...props} className={classes.myBtn}>{children}</button>;

}
~~~
# Убираем перезагрузку страницы при отправлении формы
~~~ js
const addNewPost = (e) => {

    e.preventDefault()

    console.log(title);

    console.log(description);

  };
~~~
- Где addNewPost - событие при отправлении формы
# Условная отрисовка
~~~ js
{ условие
	? если выполняется
	: если не выполняется
}
~~~
- позволяет отрисовывать теги в зависимости от каких либо условий
# Работа с сервером
## Тестовый пример
Для теста функционала можно использовать библиотеку axios и сервис JSON placeholder
~~~ js
import axios from "axios";
npm i axios
~~~
- Установка библиотеки axios и импортирование библиотеки в проект
~~~ js
  async function fetchPosts() {

    const response = await axios.get(

      "https://jsonplaceholder.typicode.com/posts"

    );
~~~
- Пример запроса
- async - обозначает что функция возвращает promise 
- promise - используются для управления асинхронными операциями - такими, которые своим выполнением не останавливают выполнения последующего кода.
- await - используется только внутри асинхронных функций, заставляет интерпретатор ждать возвращения promise (нельзя использовать в обычных функциях)
# Что такое API
API - набор данных, которые можно использовать для загрузки react-приложений
Для использования данных на стороне клиента можно использоваться API-интерфейсы# React route
~~~ js
npm i react-router-dom
~~~
- подключение библиотеки 
Чтобы производить роутинг по странице нужно импортировать
~~~ js
import { BrowserRouter, Route, Routes } from "react-router-dom";
~~~
роутинг производиться таким образом
## Пример
~~~ js
<BrowserRouter>

      <Routes>

        <Route path="/about" element={<About></About>}></Route>

      </Routes>

</BrowserRouter>
~~~
- где path - путь по которому мы можем получить доступ к странице
- element - компонент в виде страницы к которую мы хотим отрисовать
Если мы хотим чтобы проверялся вариант того случая, когда пользователь ввел неправильную ссылку то нужно написать:
~~~ js
<Route path="*" element={<Posts></Posts>}></Route>
~~~
## Переход по нажатию
Для организации перехода по ссылке нужно использовать тег Link
Данный способ предпочтительнее в том случае, когда нам необходимо организовать переход без обновления страницы
~~~ js
 <Link to="/about">о сайте</Link>
~~~
- пример использования
- to - отвечает за то куда мы отправляемся (должен совпадать с настроенным route)
# Отрисовка нескольких компонентов в цикле внутри компонента
Циклы типа  for и while не работают для отрисовки компонентов
Нужно использовать  метод map из набора методов массива
~~~ js
{arr.map((index) =>
    <Item key={index} />
)}
~~~
# Модальное окно

~~~ js

~~~