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
~~~
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
~~~
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
~~~
  const sortPost = (sort) => {

    setSelectedSort(sort);

    setPosts([...posts].sort((a,b)=> a[sort].localeCompare(b[sort])))

  };
~~~
- где сорт это название параметров по которым по хотим отсортировать данные
## useEffect
## useRef
С помощью этого хука мы можем получить доступ к DOM элементу
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
~~~
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

# Компоненты
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
## Тестовый сервер
Для теста функционала можно использовать библиотеку axios
~~~
npm i axios
~~~
- Установка библиотеки axios