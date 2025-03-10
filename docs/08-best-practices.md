# Лучшие практики

В этом разделе собраны рекомендации и лучшие практики по работе с Runlify, основанные на реальном опыте разработки проектов.

## Организация кода

### Структура метаданных

1. **Разделение на модули**
   ```typescript
   // src/meta/index.ts
   import addCatalogs from './addCatalogs';
   import addMenu from './addMenu';
   import addWscEntities from './addWscEntities';
   
   export const configureMeta = (system: SystemMetaBuilder) => {
     addCatalogs(system);
     addMenu(system);
     addWscEntities(system);
   };
   ```

2. **Группировка связанных сущностей**
   ```typescript
   // src/meta/catalogs/index.ts
   export * from './teams';
   export * from './clubs';
   export * from './players';
   export * from './matches';
   ```

3. **Отдельные файлы для больших сущностей**
   ```typescript
   // src/meta/catalogs/teams/index.ts
   export * from './team.entity';
   export * from './team.permissions';
   export * from './team.views';
   ```

### Именование

1. **Сущности**
   - Используйте существительные в единственном числе
   - Начинайте с заглавной буквы
   - Избегайте сокращений

   ```typescript
   // Хорошо
   const team = system.addCatalog('team');
   const player = system.addCatalog('player');

   // Плохо
   const teams = system.addCatalog('teams');
   const plr = system.addCatalog('plr');
   ```

2. **Поля**
   - Используйте camelCase
   - Делайте имена описательными
   - Следуйте единому стилю

   ```typescript
   // Хорошо
   entity.addScalarField('firstName', 'string');
   entity.addScalarField('dateOfBirth', 'date');

   // Плохо
   entity.addScalarField('first_name', 'string');
   entity.addScalarField('DOB', 'date');
   ```

3. **Связи**
   - Используйте понятные имена для связей
   - Указывайте тип связи
   - Добавляйте описательные заголовки

   ```typescript
   // Хорошо
   entity.addLinkField('managers', 'managerId', 'Менеджер');
   entity.addLinkField('clubs', 'clubId', 'Клуб');

   // Плохо
   entity.addLinkField('mgr', 'mgrId', 'Менеджер');
   entity.addLinkField('clb', 'clbId', 'Клуб');
   ```

## Работа с типами

### Определение полей

1. **Правильный выбор типа**
   ```typescript
   // Хорошо
   entity.addScalarField('age', 'number')
     .setRequired();

   entity.addScalarField('email', 'string')
     .setRequired()
     .setUnique();

   // Плохо
   entity.addScalarField('age', 'string');
   entity.addScalarField('email', 'text');
   ```

2. **Валидация данных**
   ```typescript
   entity.addScalarField('phone', 'string')
     .setRequired()
     .setPattern('^\\+?[1-9][0-9]{7,14}$');
   ```

3. **Значения по умолчанию**
   ```typescript
   entity.addScalarField('isActive', 'boolean')
     .setDefault(true);

   entity.addScalarField('createdAt', 'date')
     .setDefault('NOW()');
   ```

## Связи между сущностями

### Определение связей

1. **Явное указание связей**
   ```typescript
   // Хорошо
   team.addLinkField('players', 'playerId', 'Игрок')
     .setRequired();

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

3. **Обязательные связи**
   ```typescript
   entity.addLinkField('managers', 'managerId', 'Менеджер')
     .setRequired();
   ```

## Безопасность

### Права доступа

1. **Явное указание прав**
   ```typescript
   entity.setCreatableByUser(true);
   entity.setUpdatableByUser(true);
   entity.setDeletable(false);
   ```

2. **Разрешения**
   ```typescript
   entity.addPermission('canApprove', 'approve');
   entity.addPermission('canReject', 'reject');
   ```

3. **Аудит изменений**
   ```typescript
   entity.enableAudit();
   ```

## Производительность

### Оптимизация запросов

1. **Сортировка по умолчанию**
   ```typescript
   entity.setSort('name', 'ASC');
   ```

2. **Поиск**
   ```typescript
   entity.enableSearch();
   ```

3. **Пагинация**
   ```typescript
   // В REST API
   GET /api/entity?page=1&per_page=20

   // В GraphQL
   query {
     entities(page: 1, perPage: 20) {
       items {
         id
         name
       }
       total
     }
   }
   ```

## Пользовательский интерфейс

### React Admin

1. **Списки**
   ```typescript
   <List>
     <Datagrid>
       <TextField source="name" />
       <ReferenceField source="managerId" reference="managers">
         <TextField source="name" />
       </ReferenceField>
     </Datagrid>
   </List>
   ```

2. **Формы**
   ```typescript
   <Create>
     <SimpleForm>
       <TextInput source="name" />
       <ReferenceInput source="managerId" reference="managers">
         <TextInput source="name" />
       </ReferenceInput>
     </SimpleForm>
   </Create>
   ```

3. **Фильтры**
   ```typescript
   <Filter>
     <TextInput source="name" />
     <ReferenceInput source="managerId" reference="managers">
       <TextInput source="name" />
     </ReferenceInput>
   </Filter>
   ```

## Следующие шаги

- Ознакомьтесь с примерами использования в разделе [Примеры](07-examples.md)
- При возникновении проблем обратитесь к разделу [Устранение неполадок](09-troubleshooting.md) 