# Типы сущностей

В этом разделе описаны различные типы сущностей, доступные в Runlify.

## Документ

Документ - это сущность, которая представляет собой запись с набором полей и возможностью проведения.

```typescript
const document = system.addDocument('order')
  .setTitle('Заказ')
  .addScalarField('number', 'string')
  .setRequired('number')
  .addScalarField('date', 'date')
  .setRequired('date')
  .addLinkField('customer', 'customerId', 'Клиент')
  .setRequired('customerId')
  .enableAudit();
```

### Особенности
- Имеет номер и дату
- Может быть проведен
- Поддерживает аудит изменений
- Может иметь связанные документы

## Регистр накоплений

Регистр накоплений - это сущность для хранения и анализа количественных показателей.

```typescript
const register = system.addSavingsRegister('sales')
  .setTitle('Продажи')
  .addScalarField('quantity', 'number')
  .setRequired('quantity')
  .addScalarField('amount', 'number')
  .setRequired('amount')
  .addLinkField('product', 'productId', 'Товар')
  .setRequired('productId')
  .enableAudit();
```

### Особенности
- Хранит количественные показатели
- Поддерживает агрегацию данных
- Имеет измерения и ресурсы
- Может быть использован для отчетов

## Регистр сведений

Регистр сведений - это сущность для хранения дополнительной информации о других сущностях.

```typescript
const register = system.addInfoRegister('customerContacts')
  .setTitle('Контакты клиентов')
  .addScalarField('phone', 'string')
  .setRequired('phone')
  .addScalarField('email', 'string')
  .setRequired('email')
  .addLinkField('customer', 'customerId', 'Клиент')
  .setRequired('customerId')
  .enableAudit();
```

### Особенности
- Хранит дополнительную информацию
- Может иметь несколько записей для одной сущности
- Поддерживает версионирование
- Имеет измерения и ресурсы

## Создание сущности

### Базовые шаги
1. Выберите тип сущности
2. Установите заголовок
3. Добавьте необходимые поля
4. Настройте права доступа
5. Включите нужные функции (аудит, поиск и т.д.)

### Пример создания
```typescript
// Создание документа
const order = system.addDocument('order')
  .setTitle('Заказ')
  .addScalarField('number', 'string')
  .setRequired('number')
  .addScalarField('date', 'date')
  .setRequired('date')
  .enableAudit()
  .enableSearch();

// Создание регистра накоплений
const sales = system.addSavingsRegister('sales')
  .setTitle('Продажи')
  .addScalarField('quantity', 'number')
  .setRequired('quantity')
  .addScalarField('amount', 'number')
  .setRequired('amount')
  .enableAudit();

// Создание регистра сведений
const contacts = system.addInfoRegister('contacts')
  .setTitle('Контакты')
  .addScalarField('phone', 'string')
  .setRequired('phone')
  .addScalarField('email', 'string')
  .setRequired('email')
  .enableAudit();
```

## Удаление сущности

### Безопасное удаление
```typescript
// Проверка существования
if (system.hasEntity('order')) {
  system.removeEntity('order');
}
```

### Запрет удаления
```typescript
// Запрет удаления для всех
entity.setDeletable(false);
```

## Изменение сущности

### Переименование
```typescript
// Изменение названия для пользователя
entity.setTitle('Новое название');
```

### Добавление полей
```typescript
// Добавление нового поля
entity.addScalarField('newField', 'string')
  .setRequired('newField');
```

### Удаление полей
```typescript
// Удаление поля
entity.removeField('oldField');
```

## Следующие шаги

- Ознакомьтесь с лучшими практиками в разделе [Лучшие практики](08-best-practices.md)
- Изучите примеры использования в разделе [Примеры](07-examples.md)
- При возникновении проблем обратитесь к разделу [Устранение неполадок](09-troubleshooting.md) 