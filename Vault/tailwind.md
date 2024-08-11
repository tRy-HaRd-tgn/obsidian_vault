Идея tailwind заключается в том чтобы писать стили прямо в директиву class или же className. Технически это похоже на использования inline class'ов 
# Что такое ?
tailwind - css фреймворк с открытым исходным кодом
# Установка 
## 1. Установите Tailwind CSS
Установите `tailwindcss` через npm и создайте файл `tailwind.config.js` .
~~~js
npm install -D tailwindcssnpx tailwindcss init
~~~
## 2. Настройте пути к шаблонам
Добавьте пути ко всем вашим файлам шаблонов в файл `tailwind.config.js`.
~~~ js
/** @type {import('tailwindcss').Config} */ 
module.exports = { 
	content: ["./src/**/*.{html,js}"],
	theme: { 
	extend: {}, 
	},
	plugins: [],
}
~~~
## 3. Добавьте директивы Tailwind в свой CSS
Добавьте директивы `@tailwind` для каждого макета Tailwind в свой основной файл CSS.
~~~ js
@tailwind base; 
@tailwind components; 
@tailwind utilities;
~~~
## 4. Запустите процесс сборки Tailwind CLI
Запустите инструмент CLI, чтобы просканировать файлы шаблонов на предмет классов и создать свой CSS.
~~~js
npx tailwindcss -i ./src/input.css -o ./src/output.css --watch
~~~
## 5. Начните использовать Tailwind в своем HTML
Добавьте свой скомпилированный файл CSS в `<head>` и начните использовать классы утилиты Tailwind для стилизации вашего контента.
~~~ html
<!doctype html> 
<html>
<head> 
	<meta charset="UTF-8"> 
	<meta name="viewport" content="width=device-width, initial-scale=1.0"> 
	<link href="./output.css" rel="stylesheet"> 
</head> 
	<body> 
		<h1 class="text-3xl font-bold underline"> 
		Привет мир! 
		</h1> 
	</body> 
</html>
~~~