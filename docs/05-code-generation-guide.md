# Руководство по кодогенерации

В этом разделе мы рассмотрим, как использовать возможности кодогенерации Runlify для создания полнофункциональных приложений.

## Основные концепции

### SystemMetaBuilder

Основной класс для описания всей системы. Через него создаются все сущности и настраиваются их взаимосвязи.

```typescript
import { SystemMetaBuilder } from 'runlify';

export function addCatalogs(meta: SystemMetaBuilder) {
  // Здесь описываются сущности
  return { /* возвращаем созданные сущности */ };
}
```

### CatalogBuilder

Класс для описания сущностей (каталогов) в системе.

```typescript
const users = meta.addCatalog('users')
  .setTitle('Пользователи')
  .addScalarField('email', 'string')
  .setRequired('email')
  .setUnique('email')
  .addScalarField('name', 'string')
  .setTitleField('name')
  .enableSearch()
  .enableAudit();
```

## Работа с сущностями

### Создание сущности

```typescript
// Создание базовой сущности
const teams = meta.addCatalog('teams')
  .setTitle('Команды');

// Добавление полей
teams.addScalarField('name', 'string')
  .setTitleField('name')
  .setRequired('name');

// Добавление связей
teams.addLinkField('managers', 'managerId', 'Менеджер')
  .setRequired('managerId');
```

### Добавление полей

```typescript
// Скалярные поля
entity.addScalarField('title', 'string')
  .setTitleField('title')
  .setRequired('title');

// Связи
entity.addLinkField('parent', 'parentId', 'Родитель')
  .setRequired('parentId');

// Аудит
entity.enableAudit();
```

### Связи между сущностями

```typescript
// Связь один-к-одному
entity.addLinkField('profile', 'profileId', 'Профиль')
  .setRequired('profileId');

// Связь один-ко-многим
entity.addLinkField('items', 'itemId', 'Элементы')
  .setRequired('itemId');

// Связь с файлами
entity.addLinkField('files', 'fileId', 'Файлы');
```

### Настройка прав доступа

```typescript
// Ограничение создания
entity.setCreatableByUser(false);

// Ограничение обновления
entity.setUpdatableByUser(false);

// Ограничение удаления
entity.setDeletable(false);

// Добавление специфичных прав
entity.addPermission('canApprove', 'approve');
```

### Настройка поиска и фильтрации

```typescript
// Включение поиска
entity.enableSearch();

// Установка полей для поиска
entity.addScalarField('name', 'string')
  .setSearchable(true);

// Настройка сортировки по умолчанию
entity.setSort('createdAt', 'DESC');
```

## Генерация API

### REST API

Runlify автоматически генерирует следующие endpoints для каждой сущности:

```typescript
// GET /api/entity - Получение списка
// GET /api/entity/:id - Получение по ID
// POST /api/entity - Создание
// PUT /api/entity/:id - Обновление
// DELETE /api/entity/:id - Удаление
```

### GraphQL API

Также генерируются GraphQL типы и резолверы:

```graphql
type Entity {
  id: ID!
  name: String!
  createdAt: DateTime!
  updatedAt: DateTime!
}

type Query {
  entities: [Entity!]!
  entity(id: ID!): Entity
}

type Mutation {
  createEntity(input: CreateEntityInput!): Entity!
  updateEntity(id: ID!, input: UpdateEntityInput!): Entity!
  deleteEntity(id: ID!): Boolean!
}
```

### Кастомные методы

```typescript
// Добавление кастомного метода
entity.addMethod('approve', 'update', 'Одобрить')
  .setNeedFor('Метод для одобрения записи');
```

## Генерация Frontend

### React Admin компоненты

Runlify автоматически генерирует следующие компоненты:

1. **Списки**
```typescript
// Таблица с данными
entity.setListView()
  .addColumn('name', 'Имя')
  .addColumn('status', 'Статус');
```

2. **Формы**
```typescript
// Форма создания/редактирования
entity.setFormView()
  .addField('name')
  .addField('status');
```

3. **Фильтры**
```typescript
// Панель фильтрации
entity.setFilterView()
  .addFilter('status')
  .addFilter('createdAt');
```

## Примеры использования

### Базовая сущность

```typescript
const users = meta.addCatalog('users')
  .setTitle('Пользователи')
  .addScalarField('email', 'string')
  .setRequired('email')
  .setUnique('email')
  .addScalarField('name', 'string')
  .setTitleField('name')
  .addScalarField('isActive', 'boolean')
  .setDefault('isActive', true)
  .enableSearch()
  .enableAudit();
```

### Сущность со связями

```typescript
const teams = meta.addCatalog('teams')
  .setTitle('Команды')
  .addScalarField('name', 'string')
  .setTitleField('name')
  .setRequired('name')
  .addLinkField('managers', 'managerId', 'Менеджер')
  .setRequired('managerId')
  .addLinkField('clubs', 'clubId', 'Клуб')
  .setRequired('clubId')
  .addLinkField('files', 'logoId', 'Логотип команды')
  .enableSearch()
  .enableAudit();
```

## Лучшие практики

1. **Именование**
   - Используйте осмысленные имена для сущностей и полей
   - Следуйте единому стилю именования
   - Добавляйте описания на разных языках

2. **Структура**
   - Группируйте связанные сущности
   - Выделяйте общие части в отдельные сущности
   - Используйте правильные типы связей

3. **Безопасность**
   - Всегда указывайте права доступа
   - Проверяйте обязательные поля
   - Используйте валидацию данных

4. **Производительность**
   - Правильно настраивайте индексы
   - Оптимизируйте запросы
   - Используйте кэширование

## Следующие шаги

- Изучите [API Reference](./06-api-reference.md) для подробного описания всех доступных методов
- Посмотрите [примеры использования](./07-examples.md) для более сложных сценариев
- Ознакомьтесь с [лучшими практиками](./08-best-practices.md) для эффективной работы с Runlify 