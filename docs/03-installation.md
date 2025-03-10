# Установка и настройка

В этом разделе описан процесс установки и настройки Runlify для разработки проектов.

## Системные требования

### Обязательные компоненты

- Node.js (версия 16 или выше)
- yarn (npm не поддерживается)
- Git
- Docker и Docker Compose
- PostgreSQL (версия 14 или выше)
- Prisma CLI

### Рекомендуемые инструменты

- Visual Studio Code с расширениями:
  - TypeScript
  - ESLint
  - Prettier
  - GitLens
  - Prisma
- DBeaver или pgAdmin (для работы с базой данных)

## Пошаговая установка

### 1. Установка Node.js

#### Windows
1. Скачайте установщик с [официального сайта](https://nodejs.org/)
2. Запустите установщик и следуйте инструкциям
3. Проверьте установку:
   ```bash
   node --version
   yarn --version
   ```

#### Linux (Ubuntu/Debian)
```bash
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt-get install -y nodejs
```

#### macOS
```bash
brew install node
```

### 2. Установка Runlify и зависимостей

```bash
# Локальная установка в проект
yarn add runlify@latest typescript @types/node prisma @prisma/client
```

### 3. Настройка окружения

#### Создание рабочей директории
```bash
mkdir my-project
cd my-project
yarn init -y
```

#### Установка зависимостей
```bash
yarn add runlify@latest typescript @types/node prisma @prisma/client
```

## Конфигурация

### Основные настройки

Создайте файл `runlify.json` в корне проекта:

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "My Runlify Project",
  "database": {
    "type": "postgresql",
    "url": "postgresql://postgres:postgres@localhost:5432/myproject"
  },
  "generation": {
    "output": "./src/generated",
    "language": "typescript"
  }
}
```

### Настройка Prisma

1. Инициализируйте Prisma:
```bash
npx prisma init
```

2. Настройте `prisma/schema.prisma`:
```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
```

3. Создайте `.env` файл:
```env
DATABASE_URL="postgresql://postgres:postgres@localhost:5432/myproject"
```

### Настройка Docker

1. Создайте `compose/docker-compose.yml`:
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

2. Добавьте скрипты в package.json:
```json
{
  "scripts": {
    "compose:start": "docker compose -f compose/docker-compose.yml up -d",
    "compose:stop": "docker compose -f compose/docker-compose.yml stop",
    "compose:delete": "docker compose -f compose/docker-compose.yml down --volumes",
    "prisma:gen": "prisma generate",
    "prisma:newMigration": "runlify start env=migration prisma migrate dev --preview-feature"
  }
}
```

### Настройка окружения разработки

#### Visual Studio Code

1. Установите рекомендуемые расширения:
   ```json
   {
     "recommendations": [
       "dbaeumer.vscode-eslint",
       "esbenp.prettier-vscode",
       "eamodio.gitlens",
       "Prisma.prisma"
     ]
   }
   ```

2. Настройте форматирование кода:
   ```json
   {
     "editor.formatOnSave": true,
     "editor.defaultFormatter": "esbenp.prettier-vscode",
     "typescript.updateImportsOnFileMove.enabled": "always"
   }
   ```

#### ESLint и Prettier

1. Установите зависимости:
   ```bash
   yarn add -D eslint prettier @typescript-eslint/parser @typescript-eslint/eslint-plugin
   ```

2. Создайте `.eslintrc.js`:
   ```javascript
   module.exports = {
     parser: '@typescript-eslint/parser',
     plugins: ['@typescript-eslint'],
     extends: [
       'eslint:recommended',
       'plugin:@typescript-eslint/recommended'
     ]
   };
   ```

3. Создайте `.prettierrc`:
   ```json
   {
     "singleQuote": true,
     "trailingComma": "es5",
     "printWidth": 100,
     "tabWidth": 2,
     "semi": true
   }
   ```

## Структура проекта

После установки и настройки, ваш проект должен иметь следующую структуру:

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
│   └── config/
├── prisma/
│   └── schema.prisma
├── package.json
├── tsconfig.json
├── runlify.json
├── .eslintrc.js
├── .prettierrc
├── .env
└── README.md
```

## Проверка установки

1. Запустите базу данных:
```bash
yarn compose:start
```

2. Создайте тестовую сущность:
```typescript
// src/meta/addCatalogs.ts
import { SystemMetaBuilder } from 'runlify';

export function addCatalogs(meta: SystemMetaBuilder) {
  const users = meta.addCatalog('users')
    .setTitle('Пользователи')
    .addScalarField('email', 'string')
    .setRequired('email')
    .setUnique('email')
    .addScalarField('name', 'string')
    .setTitleField('name')
    .enableSearch()
    .enableAudit();

  return { users };
}
```

3. Сгенерируйте код:
```bash
yarn regen
```

4. Запустите приложение:
```bash
yarn dev:local
```

## Типичные проблемы

### Проблема: Ошибка при установке зависимостей

**Решение:**
```bash
yarn cache clean
yarn install
```

### Проблема: Ошибка подключения к базе данных

**Решение:**
1. Проверьте статус Docker контейнеров:
   ```bash
   docker ps
   ```
2. Проверьте логи контейнера:
   ```bash
   docker logs my-project-db-1
   ```
3. Убедитесь, что в `.env` файле указан правильный URL базы данных

### Проблема: Ошибка Prisma

**Решение:**
1. Проверьте схему Prisma:
   ```bash
   npx prisma validate
   ```
2. Сгенерируйте клиент:
   ```bash
   yarn prisma:gen
   ```
3. Примените миграции:
   ```bash
   yarn prisma:newMigration
   ```

## Следующие шаги

- Изучите [структуру проекта](./04-project-structure.md)
- Познакомьтесь с [руководством по кодогенерации](./05-code-generation-guide.md)
- Посмотрите [примеры использования](./07-examples.md) 