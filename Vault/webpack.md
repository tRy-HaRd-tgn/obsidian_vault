# Что такое webpack и зачем нужен ?
Webpack - сборщик модулей, он анализирует модули приложения, создает граф зависимостей, затем собирает модули в правильном порядке в один или более bundle.
# Какие решаются проблемы
Когда мы пишем код на java script и код разделяется на несколько частей, нам отдельно необходимо подключать все эти файлы да ещё и в правильном порядке, webpack решает именно такие задачи
# Установка webpack
~~~ js
npm i webpack webpack-cli -D
~~~
- Для работы webpack необходимо установить webpack и webpack-cli
# Точка входа
Сколько бы модулей не содержало приложение, всегда имеется одна точка входа. В таком модуле содержаться все остальные модули. Обычно этой точкой входа является файл index.js
Если мы сообщим webpack путь до этого файла, он используется его для создания графа зависимостей приложения. Для этого необходимо добавить свойство entry.
~~~ js
module.exports = {entry: './app/index.js'}
~~~
После создания точки входа нужно сообщить webpack о том какие преобразования он должен совершить перед преобразованием в bundle, это производиться с помощью лоадеров
~~~ js
import auth from './api/auth' // 
import config from './utils/config.json' // 
import './styles.css' // ️
import logo from './assets/logo.svg' // ️
~~~