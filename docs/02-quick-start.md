# Быстрый старт

Это руководство поможет вам быстро начать работу с Runlify и создать ваш первый проект.

## Минимальные требования

Перед началом работы убедитесь, что у вас установлено:

- Node.js (версия 16 или выше)
- yarn (npm не поддерживается)
- Git
- Docker и Docker Compose
- TypeScript
- Редактор кода (рекомендуется VS Code)

## Создание нового проекта

1. Создайте новую директорию для проекта и инициализируйте его:

```bash
mkdir my-project
cd my-project
yarn init -y
```

2. Добавьте Runlify и основные зависимости:

```bash
yarn add runlify@latest typescript @types/node prisma @prisma/client
```

3. Создайте базовую структуру проекта:

```
my-project/
├── compose/
│   └── docker-compose.yml
├── src/
│   ├── meta/
│   │   ├── addCatalogs.ts
│   │   ├── addMenu.ts
│   │   └── regenBasedOnMeta.ts
│   ├── rest/
│   │   ├── healthRouter.ts
│   │   └── restRouter.ts
│   ├── generated/
│   └── index.ts
├── prisma/
│   └── schema.prisma
├── package.json
├── tsconfig.json
└── runlify.json
```

4. Настройте Docker Compose для базы данных. Создайте файл `compose/docker-compose.yml`:

```yaml
version: '3.8'
services:
  db:
    image: postgres:14
    environment:
      POSTGRES_DB: myproject
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

5. Добавьте скрипты в package.json:

```json
{
  "scripts": {
    "compose:start": "docker compose -f compose/docker-compose.yml up -d",
    "compose:stop": "docker compose -f compose/docker-compose.yml stop",
    "compose:delete": "docker compose -f compose/docker-compose.yml down --volumes",
    "build": "(rm -rf dist || true) && tsc",
    "dev": "ts-node-dev --files src/index.ts",
    "dev:local": "runlify start env=local yarn dev",
    "prisma:gen": "prisma generate",
    "prisma:newMigration": "runlify start env=migration prisma migrate dev --preview-feature",
    "init:base": "ts-node src/init/baseInit.ts",
    "init:permissions": "yarn ts-node:withContext src/init/roles/initRolesWithPermissions.ts",
    "regen": "yarn ts-node src/meta/regenBasedOnMeta.ts && runlify regen"
  }
}
```

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