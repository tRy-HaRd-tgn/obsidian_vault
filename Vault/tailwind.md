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
## 5. Начните использовать Tailwind в своем HTML