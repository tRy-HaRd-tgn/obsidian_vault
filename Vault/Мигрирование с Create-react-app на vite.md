[Статья](https://dev.to/henriquejensen/migrating-from-create-react-app-to-vite-a-quick-and-easy-guide-5e72)
# 1 шаг
первым делом нужно удалить  create-react-app со своего проекта
~~~ terminal
npm uninstall react-scripts
~~~
# 2 шаг
установка необходимых зависимостей 
~~~ terminal
npm install vite @vitejs/plugin-react-swc vite-tsconfig-paths vite-plugin-svgr
~~~
Это сугубо пример, если нужно можно выбрать другой плагин с официального сайта vite _[documentation.](https://main.vitejs.dev/plugins/)
# 3 шаг
create-react-app использует public/index.html как точка входа по умолчанию, тогда как vite ищет index.html в корневом каталоге
~~~ html
<!-- index.html -->
<body>
  <noscript>You need to enable JavaScript to run this app.</noscript>
  <div id="root"></div>

  <script type="module" src="/src/index.tsx"></script>
</body>
~~~
# 4 шаг
добавление vite.config.ts в ваш проект со следующим контентом
~~~ ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react-swc'

// https://vitejs.dev/config/
export default defineConfig({
  base: '/',
  plugins: [react()]
})
~~~
# 5 шаг

# 6 шаг
# 7 шаг
# 8 шаг
# 9 шаг

