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
