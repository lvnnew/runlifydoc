# Устранение неполадок

В этом разделе описаны типичные проблемы, с которыми можно столкнуться при работе с Runlify, и способы их решения.

## Проблемы установки

### Ошибка при установке пакетов

**Проблема:**
```bash
npm ERR! code EACCES
npm ERR! syscall access
npm ERR! path /usr/local/lib/node_modules
```

**Решение:**
1. Используйте nvm для управления Node.js:
   ```bash
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
   nvm install node
   nvm use node
   ```

2. Или измените права доступа:
   ```bash
   sudo chown -R $USER /usr/local/lib/node_modules
   ```

### Конфликты версий

**Проблема:**
```bash
Error: Cannot find module '@prisma/client'
```

**Решение:**
1. Проверьте версии зависимостей:
   ```bash
   npm list @prisma/client
   ```

2. Очистите кэш npm:
   ```bash
   npm cache clean --force
   ```

3. Переустановите пакеты:
   ```bash
   rm -rf node_modules
   npm install
   ```

## Проблемы генерации кода

### Ошибки в метаданных

**Проблема:**
```typescript
Error: Invalid metadata configuration: Title field is required for entity "team"
```

**Решение:**
1. Проверьте наличие обязательных полей:
   ```typescript
   const team = system.addCatalog('team')
     .setTitle('Команды')
     .addScalarField('name', 'string')
     .setTitleField('name')
     .setRequired('name');
   ```

2. Убедитесь, что все связи корректны:
   ```typescript
   // Проверьте существование связанной сущности
   const teams = system.addCatalog('teams');
   const players = system.addCatalog('players');

   players.addLinkField('teams', 'teamId', 'Команда')
     .setRequired('teamId');
   ```

### Ошибки при генерации API

**Проблема:**
```bash
Error: Could not generate API endpoints: Duplicate route "/api/teams"
```

**Решение:**
1. Проверьте уникальность имен сущностей:
   ```typescript
   // Используйте уникальные имена
   const teams = system.addCatalog('teams');
   const adminTeams = system.addCatalog('adminTeams');
   ```

2. Проверьте конфигурацию маршрутов:
   ```typescript
   // В src/rest/restRouter.ts
   router.use('/api/teams', teamsRouter);
   ```

## Проблемы с базой данных

### Ошибки миграций Prisma

**Проблема:**
```bash
Error: Migration failed: Column "status" already exists
```

**Решение:**
1. Откатите последнюю миграцию:
   ```bash
   yarn prisma migrate reset
   ```

2. Исправьте схему и сгенерируйте новую миграцию:
   ```bash
   yarn prisma migrate dev
   ```

### Проблемы с соединением

**Проблема:**
```bash
Error: Could not connect to database
```

**Решение:**
1. Проверьте конфигурацию в `.env`:
   ```
   DATABASE_URL="postgresql://user:password@localhost:5432/db"
   ```

2. Убедитесь, что база данных запущена:
   ```bash
   docker ps | grep postgres
   ```

## Проблемы с frontend

### Ошибки компиляции

**Проблема:**
```bash
Error: Cannot find module '@mui/material'
```

**Решение:**
1. Установите недостающие зависимости:
   ```bash
   yarn add @mui/material @mui/icons-material @emotion/react @emotion/styled
   ```

2. Проверьте конфигурацию TypeScript:
   ```json
   {
     "compilerOptions": {
       "esModuleInterop": true,
       "allowSyntheticDefaultImports": true
     }
   }
   ```

### Проблемы с React Admin

**Проблема:**
```typescript
Error: Cannot find resource "teams"
```

**Решение:**
1. Проверьте регистрацию ресурсов:
   ```typescript
   // В src/App.tsx
   import { Admin, Resource } from 'react-admin';
   import { TeamsList } from './components/TeamsList';

   export const App = () => (
     <Admin dataProvider={dataProvider}>
       <Resource name="teams" list={TeamsList} />
     </Admin>
   );
   ```

2. Убедитесь, что API endpoints доступны:
   ```typescript
   // Проверьте доступность API
   fetch('/api/teams')
     .then(response => response.json())
     .then(data => console.log(data));
   ```

## Проблемы с правами доступа

### Ошибки авторизации

**Проблема:**
```bash
Error: Unauthorized: Cannot access resource
```

**Решение:**
1. Проверьте настройки прав:
   ```typescript
   entity.setCreatableByUser(true);
   entity.setUpdatableByUser(true);
   entity.setDeletable(false);
   entity.addPermission('canApprove', 'approve');
   ```

2. Убедитесь, что токен передается в заголовках:
   ```typescript
   const headers = {
     'Authorization': `Bearer ${token}`
   };
   ```

### Проблемы с токенами

**Проблема:**
```bash
Error: Invalid or expired token
```

**Решение:**
1. Проверьте конфигурацию аутентификации:
   ```typescript
   // В src/services/auth.ts
   const authProvider = {
     login: async (params) => {
       const response = await fetch('/api/auth/login', {
         method: 'POST',
         body: JSON.stringify(params)
       });
       const data = await response.json();
       localStorage.setItem('token', data.token);
     }
   };
   ```

2. Обновите токен:
   ```typescript
   const refreshToken = async () => {
     const response = await fetch('/api/auth/refresh', {
       method: 'POST'
     });
     const data = await response.json();
     localStorage.setItem('token', data.token);
   };
   ```

## Проблемы производительности

### Медленные запросы

**Проблема:**
Запросы к списку сущностей выполняются слишком долго.

**Решение:**
1. Используйте пагинацию:
   ```typescript
   // В REST API
   GET /api/teams?page=1&per_page=20

   // В GraphQL
   query {
     teams(page: 1, perPage: 20) {
       items {
         id
         name
       }
       total
     }
   }
   ```

2. Включите поиск для оптимизации:
   ```typescript
   entity.enableSearch();
   ```

3. Настройте сортировку по умолчанию:
   ```typescript
   entity.setSort('name', 'ASC');
   ```

## Локальная отладка на Windows

### Подготовка к отладке

1. Затягиваем изменения в runlify:
```bash
git pull
```

2. Обновляем зависимости:
```bash
yarn
```

3. Создаем и переключаемся на рабочую ветку:
```bash
git checkout -b fix-one-symbol
```

### Процесс отладки

1. В проекте runlify выполнить:
```bash
yarn link
```

2. В `package.json` из команды build удаляем `&& yarn copy templates` (после завершения отладки вернуть обратно!!!)
   > Примечание: В Linux системах этот пункт игнорируется.

3. Выполняем сборку:
```bash
yarn build
```
> Важно: выполняется после каждого нового изменения

4. В проекте, на котором будут проверяться изменения:
```bash
yarn link runlify --force
```
> Примечание: привязывается только бэкенд

5. Выполняем генерацию проекта:
```bash
yarn regen
```

### Завершение отладки

1. Отвязываем проект от runlify:
```bash
yarn unlink runlify
```

2. Отвязываем runlify:
```bash
yarn unlink
```

3. Добавляем изменения в индекс, коммитим, пушим, создаем MR и можем танцевать ча-ча-ча!

## Следующие шаги

- Ознакомьтесь с лучшими практиками в разделе [Лучшие практики](08-best-practices.md)
- Изучите примеры использования в разделе [Примеры](07-examples.md)
- Узнайте о создании кастомных элементов в разделе [Кастомные элементы](11-custom-elements.md) 