# Troubleshooting

В этом разделе описаны типичные проблемы, с которыми можно столкнуться при работе с Runlify, и способы их решения.

## Проблемы установки

### Ошибка при установке глобального пакета

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
Error: Cannot find module '@runlify/core'
```

**Решение:**
1. Проверьте версии зависимостей:
   ```bash
   npm list @runlify/core
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
Error: Invalid metadata configuration: Field "title" is required for entity "user"
```

**Решение:**
1. Проверьте наличие обязательных полей:
   ```typescript
   const user = system.addCatalog('user');
   user.addField('title', 'Название', { isTitleField: true })
     .setRequired();
   ```

2. Убедитесь, что все связи корректны:
   ```typescript
   // Проверьте существование связанной сущности
   const teams = system.addCatalog('teams');
   const players = system.addCatalog('players');

   players.addLinkField(teams, 'teamId', 'Команда');
   ```

### Ошибки при генерации API

**Проблема:**
```bash
Error: Could not generate API endpoints: Duplicate route "/api/users"
```

**Решение:**
1. Проверьте уникальность имен сущностей:
   ```typescript
   // Используйте уникальные имена
   const users = system.addCatalog('users');
   const adminUsers = system.addCatalog('adminUsers');
   ```

2. Проверьте конфигурацию маршрутов:
   ```typescript
   entity.setApiPath('/api/v1/users');
   ```

## Проблемы с базой данных

### Ошибки миграций

**Проблема:**
```bash
Error: Migration failed: Column "status" already exists
```

**Решение:**
1. Откатите последнюю миграцию:
   ```bash
   runlify migrate:revert
   ```

2. Исправьте метаданные и сгенерируйте новую миграцию:
   ```bash
   runlify generate
   runlify migrate
   ```

### Проблемы с соединением

**Проблема:**
```bash
Error: Could not connect to database
```

**Решение:**
1. Проверьте конфигурацию в `runlify.json`:
   ```json
   {
     "database": {
       "type": "postgresql",
       "url": "postgresql://user:password@localhost:5432/db"
     }
   }
   ```

2. Убедитесь, что база данных запущена:
   ```bash
   docker ps | grep postgres
   ```

## Проблемы с frontend

### Ошибки компиляции

**Проблема:**
```bash
Error: Cannot find module '@material-ui/core'
```

**Решение:**
1. Установите недостающие зависимости:
   ```bash
   npm install @material-ui/core @material-ui/icons
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

### Проблемы с формами

**Проблема:**
```typescript
Error: Form validation failed: Invalid field type
```

**Решение:**
1. Проверьте определение полей:
   ```typescript
   entity.addField('email', 'Email')
     .setType('string')
     .setPattern('^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$')
     .setValidationMessage('Неверный формат email');
   ```

2. Убедитесь, что все зависимые поля определены:
   ```typescript
   entity.addField('city', 'Город')
     .setDependsOn('country')
     .setOptionsLoader('getCitiesByCountry');
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
   entity.addPermission('canApprove', 'approve');
   ```

2. Убедитесь, что роли правильно настроены:
   ```typescript
   entity.addField('salary', 'Зарплата')
     .setVisibleForRoles(['HR', 'ADMIN']);
   ```

### Проблемы с токенами

**Проблема:**
```bash
Error: Invalid or expired token
```

**Решение:**
1. Проверьте конфигурацию аутентификации:
   ```json
   {
     "auth": {
       "tokenExpiration": "24h",
       "refreshTokenExpiration": "7d"
     }
   }
   ```

2. Обновите токен:
   ```typescript
   await auth.refreshToken(refreshToken);
   ```

## Проблемы производительности

### Медленные запросы

**Проблема:**
Запросы к списку сущностей выполняются слишком долго.

**Решение:**
1. Добавьте индексы:
   ```typescript
   entity.addField('createdAt', 'Дата создания')
     .setIndex('BTREE');
   
   entity.addUniqueConstraint(['userId', 'teamId']);
   ```

2. Оптимизируйте запросы:
   ```typescript
   entity.setDefaultPageSize(20);
   entity.setMaxPageSize(100);
   ```

### Проблемы с памятью

**Проблема:**
```bash
JavaScript heap out of memory
```

**Решение:**
1. Увеличьте лимит памяти:
   ```bash
   NODE_OPTIONS="--max-old-space-size=4096" runlify generate
   ```

2. Оптимизируйте генерацию:
   ```typescript
   system.setGenerationOptions({
     splitOutput: true,
     minify: true
   });
   ```

## Отладка

### Включение логирования

```typescript
// src/config/logger.ts
system.setLogLevel('debug');
system.setLogFormat('json');
```

### Отладка генерации

```bash
DEBUG=runlify:* runlify generate
```

### Проверка метаданных

```typescript
// src/meta/validate.ts
export const validateMeta = (system: SystemMetaBuilder) => {
  const errors = system.validate();
  if (errors.length > 0) {
    console.error('Metadata validation errors:', errors);
    process.exit(1);
  }
};
```

## Часто задаваемые вопросы

### Как обновить Runlify?

```bash
npm update -g runlify
```

### Как очистить кэш генерации?

```bash
rm -rf .runlify-cache
runlify clean
```

### Как откатиться на предыдущую версию?

```bash
npm install -g runlify@previous
```

## Следующие шаги

- Изучите [API Reference](./06-api-reference.md) для лучшего понимания возможностей
- Посмотрите [примеры использования](./07-examples.md) для изучения типичных сценариев
- Ознакомьтесь с [лучшими практиками](./08-best-practices.md) для избежания проблем 