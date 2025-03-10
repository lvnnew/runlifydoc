# Быстрый старт

В этом разделе описано как быстро начать работу с Runlify.

## Минимальные требования

- Node.js (версия 16 или выше)
- yarn (npm не поддерживается)
- Docker и Docker Compose
- Git

## Быстрый запуск проекта

1. Клонируйте репозиторий:
```bash
git clone <repository-url>
cd <project-directory>
```

2. Установите зависимости:
```bash
yarn install
```

3. Создайте и настройте `.env` файл:
```bash
cp .env.example .env
```

4. Запустите Docker контейнеры:
```bash
# Запуск контейнеров
yarn compose:start

# Проверка статуса
docker ps
```

5. Выполните инициализацию проекта:
```bash
# Инициализация с настройками по умолчанию
./initLocal.sh

# Или с указанием окружения
./initLocal.sh dev
```

6. После успешной инициализации запустите проект:
```bash
# Режим разработки
yarn dev
```

## Первый проект

### 1. Создание сущности

Создайте файл `src/meta/addCatalogs.ts`:

```typescript
import { SystemMetaBuilder } from 'runlify';

export function addCatalogs(meta: SystemMetaBuilder) {
  const users = meta.addCatalog('users')
    .setTitle('Пользователи')
    .addScalarField('email', 'string')
    .setRequired('email')
    .addScalarField('name', 'string')
    .setTitleField('name');

  return { users };
}
```

### 2. Генерация кода

```bash
yarn regen
```

### 3. Проверка результата

Откройте в браузере:
- Админ-панель: http://localhost:3000/admin
- API: http://localhost:3000/api/users

## Следующие шаги

- Изучите подробную [установку и настройку](03-installation.md)
- Ознакомьтесь со [структурой проекта](04-project-structure.md)
- Посмотрите [примеры использования](07-examples.md)

## Структура проекта

Проект состоит из двух частей:
- Бэкенд (`my-project-back`)
- Фронтенд (`my-project-front`)

### Бэкенд

Основные команды для бэкенда:

```bash
# Установка зависимостей
cd my-project-back
yarn install

# Запуск базы данных
yarn compose:start

# Разработка
yarn dev:local  # локальное окружение (порт 4000)
yarn dev:dev    # stage окружение
yarn dev:prod   # production окружение

# Работа с базой данных
yarn prisma:gen           # генерация Prisma клиента
yarn prisma:newMigration # создание новой миграции

# Генерация кода
yarn regen  # регенерация кода на основе метаданных
```

После запуска бэкенда будут доступны:
- GraphQL API: http://localhost:4000/graphql
- GraphQL Playground: http://localhost:4000/playground
- REST API: http://localhost:4000/api
- Swagger документация: http://localhost:4000/api-docs
- Метрики: http://localhost:4000/metrics

### Фронтенд

Основные команды для фронтенда:

```bash
# Установка зависимостей
cd my-project-front
yarn install

# Разработка
yarn dev       # режим разработки (порт 3000)
yarn build     # сборка для production
yarn start     # запуск production версии
```

После запуска фронтенда будет доступно:
- Веб-интерфейс: http://localhost:3000
- Административная панель: http://localhost:3000/admin

## Запуск всего проекта

1. Запустите базу данных:
```bash
cd my-project-back
yarn compose:start
```

2. В первом терминале запустите бэкенд:
```bash
cd my-project-back
yarn dev:local
```

3. Во втором терминале запустите фронтенд:
```bash
cd my-project-front
yarn dev
```

## Основные команды для разработки

### Бэкенд (my-project-back)

```bash
# Управление Docker
yarn compose:start    # запуск контейнеров
yarn compose:stop     # остановка контейнеров
yarn compose:delete   # удаление контейнеров и томов

# Разработка
yarn dev:local       # локальное окружение
yarn dev:dev         # stage окружение
yarn dev:prod        # production окружение

# База данных
yarn prisma:gen      # генерация Prisma клиента
yarn prisma:newMigration # создание миграции

# Генерация кода
yarn regen          # регенерация на основе метаданных

# Инициализация
yarn init:base      # базовая инициализация
yarn init:permissions # инициализация прав доступа

# Тестирование
yarn test           # запуск тестов
yarn lint           # проверка кода
```

### Фронтенд (my-project-front)

```bash
# Разработка
yarn dev            # режим разработки
yarn build          # сборка
yarn start          # запуск production

# Тестирование
yarn test           # запуск тестов
yarn lint           # проверка кода
```

## Что дальше?

Теперь, когда у вас есть работающее приложение, вы можете:

1. Изучить [структуру проекта](./04-project-structure.md)
2. Узнать больше о [работе с сущностями](./05-code-generation-guide.md)
3. Познакомиться с [API Reference](./06-api-reference.md)
4. Изучить [детальную структуру фреймворка Runlify](./10-runlify-structure.md)
5. Ознакомиться с [полной структурой backend проекта](./11-backend-structure.md)
6. Изучить [детальную структуру frontend проекта](./12-frontend-structure.md)

## Типичные проблемы

Если вы столкнулись с проблемами при установке или запуске, проверьте:

1. Версии Node.js и yarn
2. Статус Docker контейнеров (`docker ps`)
3. Доступность базы данных (порт 5432)
4. Логи приложения и базы данных
5. Правильность конфигурации в `runlify.json`

Более подробную информацию о решении проблем можно найти в разделе [Troubleshooting](./09-troubleshooting.md). 