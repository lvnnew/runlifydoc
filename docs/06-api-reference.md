# API Reference

## Основные классы

### SystemMetaBuilder

Основной класс для описания системы. Позволяет добавлять каталоги и настраивать их взаимосвязи.

#### Методы

- `addCatalog(name: string)`: Добавляет новый каталог
- `addMenu(name: string)`: Добавляет пункт меню
- `addWscEntity(name: string)`: Добавляет дополнительную сущность
- `regenBasedOnMeta()`: Регенерирует код на основе метаданных

### CatalogBuilder

Класс для создания каталогов.

#### Методы

- `setTitle(title: string)`: Устанавливает заголовок
- `addScalarField(name: string, type: string)`: Добавляет скалярное поле
- `addLinkField(name: string, target: string, title: string)`: Добавляет поле-ссылку
- `setTitleField(fieldName: string)`: Устанавливает поле заголовка
- `setRequired(fieldName: string)`: Делает поле обязательным
- `setUnique(fieldName: string)`: Делает поле уникальным
- `setDefault(fieldName: string, value: any)`: Устанавливает значение по умолчанию
- `enableSearch()`: Включает поиск
- `enableAudit()`: Включает аудит изменений
- `setCreatableByUser(value: boolean)`: Устанавливает возможность создания пользователем
- `setUpdatableByUser(value: boolean)`: Устанавливает возможность обновления пользователем
- `setDeletable(value: boolean)`: Устанавливает возможность удаления
- `addPermission(name: string, action: string)`: Добавляет разрешение
- `setSort(field: string, direction: 'ASC' | 'DESC')`: Устанавливает сортировку по умолчанию

## Типы полей

### Скалярные поля

Доступные типы:
- `string`: Строка
- `number`: Число
- `boolean`: Логическое значение
- `date`: Дата
- `datetime`: Дата и время
- `json`: JSON объект

### Связи

Типы связей:
- `oneToOne`: Один к одному
- `oneToMany`: Один ко многим
- `manyToMany`: Многие ко многим

## REST API

### Endpoints

Для каждой сущности автоматически генерируются следующие endpoints:

```typescript
// GET /api/entity - Получение списка
// GET /api/entity/:id - Получение по ID
// POST /api/entity - Создание
// PUT /api/entity/:id - Обновление
// DELETE /api/entity/:id - Удаление
```

### Query параметры

- `filter`: Фильтрация по полям
- `sort`: Сортировка
- `page`: Номер страницы
- `per_page`: Количество элементов на странице
- `search`: Поиск по полям

## GraphQL API

### Типы

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

### Операции

- `query`: Получение данных
- `mutation`: Изменение данных
- `subscription`: Подписка на изменения

## React Admin

### Компоненты

- `ListView`: Таблица с данными
- `FormView`: Форма создания/редактирования
- `FilterView`: Панель фильтрации

### Настройка

```typescript
// Таблица
entity.setListView()
  .addColumn('name', 'Имя')
  .addColumn('status', 'Статус');

// Форма
entity.setFormView()
  .addField('name')
  .addField('status');

// Фильтры
entity.setFilterView()
  .addFilter('status')
  .addFilter('createdAt');
```

## Примеры использования

### Создание каталога пользователей

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

### Создание каталога команд

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

## Команды для разработки

### Docker и окружение

```bash
# Запуск контейнеров
yarn compose:start

# Остановка контейнеров
yarn compose:stop

# Удаление контейнеров
yarn compose:delete
```

### Разработка

```bash
# Сборка
yarn build

# Разработка
yarn dev:local

# Генерация кода
yarn regen

# Миграции
yarn prisma:newMigration
```

## Следующие шаги

- Изучите примеры использования в разделе [Примеры](07-examples.md)
- Ознакомьтесь с лучшими практиками в разделе [Лучшие практики](08-best-practices.md)
- При возникновении проблем обратитесь к разделу [Устранение неполадок](09-troubleshooting.md) 