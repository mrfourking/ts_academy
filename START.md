# Шаги по настройке минимального проекта на typescript

## Инициализация git-репозитория

1. Создаем пустой репозиторий в гитхаб/гитлаб/другой системе контроля версий
    1. Теперь либо клонируем репозиторий на машину через команду `git clone`
    2. Либо создаем пустую папку и в ней выполняем команды
        - `git init`
        - `git remote add origin repo_host`, где *repo_host* - адрес репозитория в системе хранения версий
2. Добавляем в корень проекта файл `.gitignore`, в который добавляем строки:

```
js
node_modules
```

## Создание необходимых файлов/папок в директории проекта

1. В папке с проектом создаем папку для исходников, например `./src`
2. Далее создаем папку для скомпилированных файлов, например `./js`
3. Далее инициализируем проект через `npm init`, после чего добавляется файл *package.json*
4. Добавляем в корень проекта файл `index.html` со следующим содержанием:
   ```HTML
   <script src="js/main.js"></script>
   ```
5. В директории `./src` создаем файл *main.ts*

## Установка компилятора typescript

1. После устанавливаем компилятор typescript с помощью команды `npm i -D typescript`
2. Создаем конфигурацию typescript с помощью команды `npx tsc --init`
3. Раскомментируем флаги в `tsconfig.json` в секции *compilerOptions*:
   ```JSON
   "rootDir": "./src",
   "outDir": "./js",
   ```
4. Добавляем в корень json строку:
   ```JSON
   "include": ["./src"]
   ```
5. В файле *main.ts* добавим строку для проверки удачной компиляции, например такую
   ```TS
   document.title = `${new Date().toISOString()} TypeScript compiled`;
   ```
6. Для компиляции файла *main.ts* и получения *./js/main.js* выполним команду `npx tsc`

## Настройка дополнительных возможностей

Для удобства разработки и сохранения единого стиля можно настроить линтеры для js, css, html. Например следующим
образом:

1. ### Настройка линтера для typescript:
    1. Установим необходимые зависимости
       ```BASH
          npm install --save-dev @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint
       ```
    2. Создадим файл *.eslintrc.js* в корне проект и добавим следующее содержимое:
       ```JS
        module.exports = {
         extends: ['eslint:recommended', 'plugin:@typescript-eslint/recommended'],
         parser: '@typescript-eslint/parser',
         plugins: ['@typescript-eslint'],
         root: true,
        };
       ```
    3. Запустим команду с помощью `npx eslint .`
2. ### Настройка линтера для CSS
   1. Установим необходимые зависимости, например 
     ```BASH
      npm install --save-dev stylelint stylelint-config-css-modules stylelint-config-recess-order stylelint-config-standard
     ```
   2. Создадим файл *.stylelintrc.json* в корне проект и добавим следующее содержимое:
     ```JSON
      {
        "extends": [
          "stylelint-config-standard",
          "stylelint-config-css-modules",
          "stylelint-config-recess-order"
        ],
        "rules": {
          "indentation": 2,
          "no-descending-specificity": null,
          "comment-whitespace-inside": null,
          "comment-empty-line-before": null,
          "alpha-value-notation": "number",
          "selector-class-pattern": null,
          "color-function-notation": "legacy",
          "declaration-block-no-redundant-longhand-properties": null
        }
      }
     ```
   3. Запустим команду с помощью `npx stylelint "**/*.css"`
3. ### Настройка .editorconfig
Создаем файл *.edittorconfig* со следующим содержанием
```
; top-most EditorConfig file
root = true

; Unix-style newlines
[*]
charset = utf-8
insert_final_newline = true

[*.js]
indent_style = space
indent_size = 2

[*.css]
indent_style = space
indent_size = 2
```
4. ### live-server
Устанавливаем сервер для проверки результата и запуска локальной версии, например [serve](https://www.npmjs.com/package/serve)

## Добавление скриптов для package.json
Для сбора всех скриптов по компиляции, сборке, линтингу кода, добавляем скрипты в package.json:
```JSON
 "scripts": {
    "build": "npx tsc",
    "build:run": "serve -s ./",
    "lint:js": "npx eslint .",
    "lint:css": "npx stylelint \"**/*.css\""
}
```
