# Руководство по кодогенерации

В этом разделе мы рассмотрим, как использовать возможности кодогенерации Runlify для создания полнофункциональных приложений.

## Основные концепции

### SystemMetaBuilder

Основной класс для описания всей системы. Через него создаются все сущности и настраиваются их взаимосвязи.

```typescript
import { SystemMetaBuilder } from 'runlify';

const system = new SystemMetaBuilder();
```

### CatalogBuilder

Класс для описания сущностей (каталогов) в системе.

```typescript
const users = system.addCatalog('users');
users.setNeedFor('Таблица пользователей');
users.setTitles({
  en: { plural: 'Users', singular: 'User' },
  ru: { plural: 'Пользователи', singular: 'Пользователь' }
});
```

## Работа с сущностями

### Создание сущности

```typescript
// Создание базовой сущности
const teams = system.addCatalog('teams');

// Установка описания
teams.setNeedFor('Таблица команд');

// Установка заголовков на разных языках
teams.setTitles({
  en: { plural: 'Teams', singular: 'Team' },
  ru: { plural: 'Команды', singular: 'Команда' }
});
```

### Добавление полей

```typescript
// Простое текстовое поле
teams.addField('title', 'Название', { isTitleField: true })
  .setType('string')
  .setRequired();

// Числовое поле
teams.addField('age', 'Возраст')
  .setType('int')
  .setRequired();

// Булево поле
teams.addField('isActive', 'Активен')
  .setType('bool')
  .setDefault(true);

// Поле даты
teams.addField('createdAt', 'Дата создания')
  .setType('date')
  .setNotUpdatableByUser(undefined, 'new Date()');
```

### Связи между сущностями

```typescript
// Связь один-к-одному
users.addLinkField('profiles', 'profileId', 'Профиль')
  .setRequired();

// Связь один-ко-многим
teams.addLinkField('managers', 'managerId', 'Менеджер')
  .setRequired()
  .setNotUpdatableByUser();

// Связь с файлами
teams.addLinkField(files, 'logoId', 'Логотип команды');
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
entity.setSearchEnabled(true);

// Установка полей для поиска
entity.addField('name', 'Имя')
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

### Кастомные методы

```typescript
// Добавление кастомного метода
entity.addMethod('approve', 'update', 'Одобрить')
  .setNeedFor('Метод для одобрения записи');
```

## Генерация Frontend

### Компоненты

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
const users = system.addCatalog('users');
users.setNeedFor('Таблица пользователей');
users.setTitles({
  ru: { plural: 'Пользователи', singular: 'Пользователь' }
});

users.addField('email', 'Email', { isTitleField: true })
  .setType('string')
  .setRequired()
  .setUnique();

users.addField('name', 'Имя')
  .setType('string')
  .setRequired();

users.addField('isActive', 'Активен')
  .setType('bool')
  .setDefault(true);

users.setSearchEnabled(true);
users.setAuditable(true);
```

### Сущность со связями

```typescript
const teams = system.addCatalog('teams');
teams.setNeedFor('Таблица команд');
teams.setTitles({
  ru: { plural: 'Команды', singular: 'Команда' }
});

teams.addField('name', 'Название', { isTitleField: true })
  .setType('string')
  .setRequired();

teams.addLinkField('managers', 'managerId', 'Менеджер')
  .setRequired();

teams.addLinkField('clubs', 'clubId', 'Клуб')
  .setRequired();

teams.addLinkField(files, 'logoId', 'Логотип команды');

teams.addField('createdAt', 'Дата создания')
  .setType('date')
  .setNotUpdatableByUser(undefined, 'new Date()');
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