# Генерация проектов

## Обзор

Одной из ключевых возможностей Runlify CLI является генерация проектов на основе шаблонов. Это позволяет быстро создавать новые проекты с предварительно настроенной структурой, зависимостями и конфигурацией. В этом разделе описаны различные аспекты генерации проектов с помощью Runlify CLI.

## Инициализация проекта

Для создания нового проекта используйте команду `runlify init`:

```bash
runlify init [name] [options]
```

Параметры:
- `name` - Имя проекта (опционально). Если не указано, будет использовано имя текущей директории.

Опции:
- `--template <template>` - Шаблон проекта (по умолчанию: basic)
- `--force` - Перезаписать существующие файлы
- `--skip-install` - Пропустить установку зависимостей
- `--skip-git` - Не инициализировать Git-репозиторий
- `--verbose` - Подробный вывод

Пример:
```bash
runlify init my-awesome-project --template react
```

## Доступные шаблоны проектов

Runlify CLI предоставляет несколько предварительно настроенных шаблонов проектов:

### basic

Базовый шаблон с минимальной конфигурацией.

```bash
runlify init --template basic
```

Структура проекта:
```
my-project/
├── src/
│   └── index.ts
├── package.json
├── tsconfig.json
├── .eslintrc.js
├── .gitignore
└── README.md
```

### react

Шаблон для React-приложения с TypeScript.

```bash
runlify init --template react
```

Структура проекта:
```
my-project/
├── public/
│   ├── index.html
│   └── favicon.ico
├── src/
│   ├── components/
│   ├── pages/
│   ├── App.tsx
│   ├── index.tsx
│   └── styles.css
├── package.json
├── tsconfig.json
├── .eslintrc.js
├── .gitignore
└── README.md
```

### node-api

Шаблон для Node.js API с Express и TypeScript.

```bash
runlify init --template node-api
```

Структура проекта:
```
my-project/
├── src/
│   ├── controllers/
│   ├── models/
│   ├── routes/
│   ├── middleware/
│   ├── utils/
│   └── index.ts
├── package.json
├── tsconfig.json
├── .eslintrc.js
├── .gitignore
└── README.md
```

### fullstack

Шаблон для полноценного fullstack-приложения с React, Node.js и TypeScript.

```bash
runlify init --template fullstack
```

Структура проекта:
```
my-project/
├── client/
│   ├── public/
│   ├── src/
│   ├── package.json
│   └── tsconfig.json
├── server/
│   ├── src/
│   ├── package.json
│   └── tsconfig.json
├── package.json
├── .eslintrc.js
├── .gitignore
└── README.md
```

## Создание собственных шаблонов

Вы можете создавать собственные шаблоны проектов для использования с Runlify CLI.

### Структура шаблона

Шаблон проекта должен иметь следующую структуру:

```
my-template/
├── template/
│   ├── ... (файлы и директории шаблона)
└── runlify-template.json
```

Файл `runlify-template.json` содержит метаданные шаблона:

```json
{
  "name": "my-template",
  "version": "1.0.0",
  "description": "Мой пользовательский шаблон",
  "author": "Ваше имя",
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "typescript": "^5.0.0",
    "eslint": "^8.0.0"
  },
  "scripts": {
    "postInstall": "echo 'Установка завершена!'"
  }
}
```

### Использование переменных в шаблонах

В файлах шаблона вы можете использовать переменные, которые будут заменены при генерации проекта:

- `{{name}}` - Имя проекта
- `{{description}}` - Описание проекта
- `{{author}}` - Автор проекта
- `{{version}}` - Версия проекта
- `{{currentYear}}` - Текущий год

Пример использования переменных в файле `package.json`:

```json
{
  "name": "{{name}}",
  "version": "{{version}}",
  "description": "{{description}}",
  "author": "{{author}}",
  "license": "MIT"
}
```

### Регистрация шаблона

Для использования собственного шаблона, зарегистрируйте его в Runlify CLI:

```bash
runlify template add <path-to-template>
```

После регистрации шаблон будет доступен для использования:

```bash
runlify init --template my-template
```

## Генерация компонентов проекта

Помимо генерации целых проектов, Runlify CLI позволяет генерировать отдельные компоненты проекта с помощью команды `runlify generate` (или сокращенно `runlify g`):

```bash
runlify generate <type> <name> [options]
```

Типы генераторов:
- `component` - Генерирует компонент
- `module` - Генерирует модуль
- `service` - Генерирует сервис
- `model` - Генерирует модель данных

Примеры:
```bash
runlify generate component Button
runlify g module Auth
```

## Настройка генерации

Вы можете настроить процесс генерации проектов и компонентов в файле конфигурации проекта `runlify.json`:

```json
{
  "generators": {
    "component": {
      "path": "src/components",
      "extension": "tsx",
      "template": "functional"
    },
    "module": {
      "path": "src/modules",
      "extension": "ts"
    }
  }
}
```

## Рекомендации по генерации проектов

- Используйте предварительно настроенные шаблоны для быстрого старта проекта
- Создавайте собственные шаблоны для стандартизации проектов в вашей команде
- Настраивайте генераторы компонентов в соответствии с архитектурой вашего проекта
- Используйте переменные в шаблонах для динамической генерации контента

## Подробная документация

Для более подробной информации о возможностях генерации проектов, включая программный API, строители сущностей и расширенные примеры, смотрите раздел [Подробная документация по генерации проектов](./12-projects-generation-details.md). 