# Установка окружения

## Сложный путь

```shell
npx create-react-app # Создаем проект
yarn eject # Разбираем зависимости на более явные части
yarn add @babel/plugin-proposal-decorators mobx mobx-react --dev # Устанавливаем зависимости
# Также в уроки ставится mobx-react-devtools - но они устарели, вместо них используется браузерное расширение. 
```

Затем добавляем плагин в package.json
```json
"babel": {
"presets": [
  "react-app"
],
"plugins": [
  [
    "@babel/plugin-proposal-decorators", // Чтобы правильно транспилировать декораторы
    {
      "legacy": true
    }
  ]
]
}, 
```

Удаляем из src ненужные файлы, кроме `index.js`, `index.css`, `serviceWorker.js`
