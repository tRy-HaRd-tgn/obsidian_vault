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
добавление vite-env.d.ts
создайте vite-env.d.ts файл внутри src со следующим кодом
~~~ ts
/// <reference types="vite/client" />
~~~
# 6 шаг
добавление vite скриптов
замените CRA скрипты в  package.json на Vite скрипты
~~~ ts
 "scripts": {
    "start": "vite",
    "build": "tsc && vite build",
    "serve": "vite preview"
}
~~~
# 7 шаг
обновите tsconfig.json до последней версии
~~~ json
{ 
  "compilerOptions": {
    "target": "ESNext",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": false,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "noFallthroughCasesInSwitch": true,
    "jsx": "react-jsx",
    "types": ["vite/client", "vite-plugin-svgr/client"]
  },
  "include": ["src"]
}
~~~
# 8 шаг
поскольку мы переходим на vite, хорошая идея - рассмотреть возможность перехода с [jest](https://jestjs.io/) на vitest
## 8.1 шаг 
установите vitest зависимости 
~~~ terminal
npm i -D jsdom vitest @vitest/coverage-v8
~~~
## 8.2 шаг
обновите vite.config.ts включив в него
~~~ ts
import { defineConfig } from 'vitest/config'
import react from '@vitejs/plugin-react-swc'

// https://vitejs.dev/config/
export default defineConfig({
  base: '/',
  plugins: [react()],
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: './src/setupTests.ts',
    css: true,
    reporters: ['verbose'],
    coverage: {
      reporter: ['text', 'json', 'html'],
      include: ['src/**/*'],
      exclude: [],
    }
  },
})
~~~
## 8.3 шаг
обновите package.json vite скриптами
~~~ json
 "scripts": {
    "start": "vite",
    "build": "tsc && vite build",
    "serve": "vite preview",
    "test": "vitest",
    "test:coverage": "vitest run --coverage --watch=false"
  },
~~~
# 9 шаг
если вы используйте GitHub Actions чтобы пушить ваш код в GitHub репозиторий, вам нужно обновить файл рабочего процесса так как vite генерирует dist папку, когда мы запускаем npm run build
~~~
name: GitHub Pages

on:
  push:
    branches:
      - master
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-22.04
    permissions:
      contents: write
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - uses: actions/checkout@v3

      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: "16"

      - name: Cache dependencies
        uses: actions/cache@v3
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-

      - run: npm ci
      - run: npm run build

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/master'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
~~~

