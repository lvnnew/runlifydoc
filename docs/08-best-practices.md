# Лучшие практики

В этом разделе собраны рекомендации и лучшие практики по работе с Runlify, основанные на реальном опыте разработки проектов.

## Организация кода

### Структура метаданных

1. **Разделение на модули**
   ```typescript
   // src/meta/index.ts
   import addCatalogs from './addCatalogs';
   import addMenu from './addMenu';
   import addServices from './addServices';
   
   export const configureMeta = (system: SystemMetaBuilder) => {
     addCatalogs(system);
     addMenu(system);
     addServices(system);
   };
   ```

2. **Группировка связанных сущностей**
   ```typescript
   // src/meta/catalogs/index.ts
   export * from './users';
   export * from './teams';
   export * from './matches';
   export * from './reports';
   ```

3. **Отдельные файлы для больших сущностей**
   ```typescript
   // src/meta/catalogs/teams/index.ts
   export * from './team.entity';
   export * from './team.methods';
   export * from './team.views';
   ```

### Именование

1. **Сущности**
   - Используйте существительные в единственном числе
   - Начинайте с заглавной буквы
   - Избегайте сокращений

   ```typescript
   // Хорошо
   const User = system.addCatalog('user');
   const TeamMember = system.addCatalog('teamMember');

   // Плохо
   const users = system.addCatalog('users');
   const tm = system.addCatalog('tm');
   ```

2. **Поля**
   - Используйте camelCase
   - Делайте имена описательными
   - Следуйте единому стилю

   ```typescript
   // Хорошо
   entity.addField('firstName', 'Имя');
   entity.addField('dateOfBirth', 'Дата рождения');

   // Плохо
   entity.addField('first_name', 'Имя');
   entity.addField('DOB', 'Дата рождения');
   ```

3. **Методы**
   - Используйте глаголы для действий
   - Делайте имена понятными
   - Указывайте назначение

   ```typescript
   // Хорошо
   entity.addMethod('approveReport', 'update', 'Одобрить отчет');
   entity.addMethod('generateInvoice', 'create', 'Создать счет');

   // Плохо
   entity.addMethod('approve', 'update', 'Одобрить');
   entity.addMethod('gen', 'create', 'Создать');
   ```

## Работа с типами

### Определение полей

1. **Правильный выбор типа**
   ```typescript
   // Хорошо
   entity.addField('age', 'Возраст')
     .setType('int')
     .setMin(0)
     .setMax(150);

   entity.addField('email', 'Email')
     .setType('string')
     .setPattern('^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$');

   // Плохо
   entity.addField('age', 'Возраст')
     .setType('string');

   entity.addField('email', 'Email')
     .setType('text');
   ```

2. **Валидация данных**
   ```typescript
   entity.addField('phone', 'Телефон')
     .setType('string')
     .setRequired()
     .setPattern('^\\+?[1-9][0-9]{7,14}$')
     .setValidationMessage('Неверный формат телефона');
   ```

3. **Значения по умолчанию**
   ```typescript
   entity.addField('status', 'Статус')
     .setType('string')
     .setDefault('ACTIVE')
     .setEnum(['ACTIVE', 'INACTIVE', 'SUSPENDED']);

   entity.addField('createdAt', 'Дата создания')
     .setType('date')
     .setDefault('NOW()');
   ```

## Связи между сущностями

### Определение связей

1. **Явное указание связей**
   ```typescript
   // Хорошо
   team.addLinkField('players', 'playerId', 'Игрок')
     .setRequired()
     .setOnDelete('CASCADE');

   // Плохо
   team.addLinkField('players', 'player', 'Игрок');
   ```

2. **Двусторонние связи**
   ```typescript
   // В сущности Team
   team.addLinkField('players', 'playerId', 'Игрок');

   // В сущности Player
   player.addLinkField('teams', 'teamId', 'Команда');
   ```

3. **Каскадное удаление**
   ```typescript
   entity.addLinkField('documents', 'documentId', 'Документ')
     .setOnDelete('CASCADE')
     .setOnUpdate('CASCADE');
   ```

## Безопасность

### Права доступа

1. **Явное указание прав**
   ```typescript
   entity.setCreatableByUser(false);
   entity.setUpdatableByUser(true);
   entity.setDeletable(false);
   ```

2. **Ограничение полей**
   ```typescript
   entity.addField('internalNote', 'Внутренняя заметка')
     .setVisibleForRoles(['ADMIN', 'MANAGER']);

   entity.addField('salary', 'Зарплата')
     .setEditableForRoles(['HR', 'ADMIN']);
   ```

3. **Аудит изменений**
   ```typescript
   entity.setAuditable(true);
   entity.addField('lastModifiedBy', 'Изменено')
     .setType('string')
     .setNotUpdatableByUser();
   ```

## Производительность

### Оптимизация запросов

1. **Индексы**
   ```typescript
   entity.addField('email', 'Email')
     .setType('string')
     .setIndex('BTREE');

   entity.addField('createdAt', 'Дата создания')
     .setType('date')
     .setIndex('BTREE');
   ```

2. **Составные индексы**
   ```typescript
   entity.addUniqueConstraint(['userId', 'teamId']);
   entity.addIndex(['status', 'createdAt']);
   ```

3. **Пагинация**
   ```typescript
   entity.setDefaultPageSize(20);
   entity.setMaxPageSize(100);
   ```

## Пользовательский интерфейс

### Формы

1. **Группировка полей**
   ```typescript
   entity.setFormView()
     .addGroup('Основное', ['title', 'description'])
     .addGroup('Контакты', ['email', 'phone'])
     .addGroup('Дополнительно', ['notes']);
   ```

2. **Валидация**
   ```typescript
   entity.addField('password', 'Пароль')
     .setType('string')
     .setMinLength(8)
     .setValidationMessage('Пароль должен содержать минимум 8 символов')
     .setPattern('^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{8,}$')
     .setValidationMessage('Пароль должен содержать буквы и цифры');
   ```

3. **Зависимые поля**
   ```typescript
   entity.addField('country', 'Страна')
     .setType('string')
     .setEnum(['RU', 'US', 'UK']);

   entity.addField('city', 'Город')
     .setType('string')
     .setDependsOn('country')
     .setOptionsLoader('getCitiesByCountry');
   ```

### Списки

1. **Настройка колонок**
   ```typescript
   entity.setListView()
     .addColumn('title', 'Название')
     .addColumn('status', 'Статус')
     .addColumn('createdAt', 'Создано')
     .setDefaultSort('createdAt', 'DESC');
   ```

2. **Фильтры**
   ```typescript
   entity.setFilterView()
     .addFilter('status', 'Статус')
     .addFilter('createdAt', 'Дата создания')
     .addFilter('category', 'Категория');
   ```

3. **Действия**
   ```typescript
   entity.setListView()
     .addAction('edit', 'Редактировать')
     .addAction('delete', 'Удалить')
     .addCustomAction('approve', 'Одобрить');
   ```

## Тестирование

1. **Тестовые данные**
   ```typescript
   // src/meta/seeds/test.ts
   export const createTestData = async (ctx: Context) => {
     await ctx.prisma.users.create({
       data: {
         email: 'test@example.com',
         name: 'Test User',
         role: 'USER'
       }
     });
   };
   ```

2. **Валидация метаданных**
   ```typescript
   // src/meta/validate.ts
   export const validateMeta = (system: SystemMetaBuilder) => {
     // Проверка обязательных полей
     system.getCatalogs().forEach(catalog => {
       if (!catalog.getTitleField()) {
         throw new Error(`Catalog ${catalog.getName()} must have a title field`);
       }
     });
   };
   ```

## Документация

1. **Описание сущностей**
   ```typescript
   entity.setNeedFor('Сущность для хранения информации о пользователях системы');
   entity.setDescription('Содержит основные данные пользователя, контакты и настройки');
   ```

2. **Описание полей**
   ```typescript
   entity.addField('status', 'Статус')
     .setDescription('Текущий статус пользователя в системе')
     .setEnum(['ACTIVE', 'INACTIVE'])
     .setEnumDescriptions({
       ACTIVE: 'Пользователь активен',
       INACTIVE: 'Пользователь неактивен'
     });
   ```

3. **Примеры использования**
   ```typescript
   entity.addMethod('approve', 'update', 'Одобрить')
     .setDescription('Метод для одобрения записи модератором')
     .setExample({
       input: { id: 1, comment: 'Одобрено' },
       output: { success: true }
     });
   ```

## Следующие шаги

- Изучите [примеры использования](./07-examples.md) для практического применения этих практик
- Ознакомьтесь с разделом [Troubleshooting](./09-troubleshooting.md) для решения типичных проблем
- Посмотрите [API Reference](./06-api-reference.md) для подробного описания всех доступных методов 