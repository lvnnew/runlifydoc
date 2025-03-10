# Установка и настройка

В этом разделе описан процесс установки и настройки Runlify для разработки проектов.

## Системные требования

### Обязательные компоненты

- Node.js (версия 16 или выше)
- npm (версия 7 или выше) или yarn
- Git
- PostgreSQL (версия 12 или выше)

### Рекомендуемые инструменты

- Visual Studio Code с расширениями:
  - TypeScript
  - ESLint
  - Prettier
  - GitLens
- Docker и Docker Compose (для локальной разработки)
- DBeaver или pgAdmin (для работы с базой данных)

## Пошаговая установка

### 1. Установка Node.js

#### Windows
1. Скачайте установщик с [официального сайта](https://nodejs.org/)
2. Запустите установщик и следуйте инструкциям
3. Проверьте установку:
   ```bash
   node --version
   npm --version
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

### 2. Установка Runlify

```bash
# Глобальная установка
npm install -g runlify

# Проверка установки
runlify --version
```

### 3. Настройка окружения

#### Создание рабочей директории
```bash
mkdir my-runlify-project
cd my-runlify-project
```

#### Инициализация проекта
```bash
runlify init
```

#### Установка зависимостей
```bash
npm install
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
    "url": "postgresql://user:password@localhost:5432/db"
  },
  "generation": {
    "output": "./src/generated",
    "language": "typescript",
    "frontend": {
      "framework": "react",
      "ui": "material-ui"
    }
  }
}
```

### Настройка базы данных

#### Локальная база данных

1. Установите PostgreSQL:
   ```bash
   # Ubuntu/Debian
   sudo apt-get install postgresql postgresql-contrib

   # macOS
   brew install postgresql
   ```

2. Создайте базу данных:
   ```bash
   createdb my_project_db
   ```

#### Docker (рекомендуется для разработки)

1. Создайте `docker-compose.yml`:
   ```yaml
   version: '3.8'
   services:
     db:
       image: postgres:13
       environment:
         POSTGRES_DB: my_project_db
         POSTGRES_USER: user
         POSTGRES_PASSWORD: password
       ports:
         - "5432:5432"
       volumes:
         - postgres_data:/var/lib/postgresql/data

   volumes:
     postgres_data:
   ```

2. Запустите контейнер:
   ```bash
   docker-compose up -d
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
       "ms-vscode.vscode-typescript-tslint-plugin"
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
   npm install --save-dev eslint prettier @typescript-eslint/parser @typescript-eslint/eslint-plugin
   ```

2. Создайте `.eslintrc.js`:
   ```javascript
   module.exports = {
     parser: '@typescript-eslint/parser',
     plugins: ['@typescript-eslint'],
     extends: [
       'eslint:recommended',
       'plugin:@typescript-eslint/recommended'
     ],
     rules: {
       // Ваши правила
     }
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
my-runlify-project/
├── src/
│   ├── meta/
│   │   ├── addCatalogs.ts
│   │   ├── addMenu.ts
│   │   └── index.ts
│   ├── generated/
│   ├── rest/
│   └── config/
├── prisma/
│   └── schema.prisma
├── runlify.json
├── package.json
├── tsconfig.json
├── .eslintrc.js
├── .prettierrc
├── .gitignore
└── README.md
```

## Проверка установки

### 1. Создание тестовой сущности

```typescript
// src/meta/addCatalogs.ts
import { SystemMetaBuilder } from 'runlify';

const addCatalogs = (system: SystemMetaBuilder) => {
  const users = system.addCatalog('users');
  users.setNeedFor('Таблица пользователей');
  users.setTitles({
    ru: { plural: 'Пользователи', singular: 'Пользователь' }
  });

  users.addField('email', 'Email')
    .setType('string')
    .setRequired();

  users.addField('name', 'Имя')
    .setType('string')
    .setRequired();
};

export default addCatalogs;
```

### 2. Генерация кода

```bash
runlify generate
```

### 3. Запуск миграций

```bash
runlify migrate
```

### 4. Запуск приложения

```bash
# Запуск backend
npm run start:backend

# В новом терминале запустите frontend
npm run start:frontend
```

## Типичные проблемы

### Проблема: Ошибка при установке глобального пакета

**Решение:**
```bash
sudo npm install -g runlify
# или
npm install -g runlify --unsafe-perm
```

### Проблема: Ошибка подключения к базе данных

**Решение:**
1. Проверьте, запущен ли PostgreSQL:
   ```bash
   sudo service postgresql status
   # или
   docker ps | grep postgres
   ```

2. Проверьте конфигурацию в `runlify.json`

### Проблема: Ошибки TypeScript

**Решение:**
1. Проверьте версию TypeScript:
   ```bash
   tsc --version
   ```

2. Обновите `tsconfig.json`:
   ```json
   {
     "compilerOptions": {
       "target": "es2019",
       "module": "commonjs",
       "strict": true,
       "esModuleInterop": true,
       "skipLibCheck": true,
       "forceConsistentCasingInFileNames": true
     }
   }
   ```

## Следующие шаги

- Изучите [структуру проекта](./04-project-structure.md)
- Познакомьтесь с [руководством по кодогенерации](./05-code-generation-guide.md)
- Посмотрите [примеры использования](./07-examples.md) 