# Классы и методы

В этом разделе описаны основные классы и их методы в Runlify.

## BaseBuilder

Базовый класс для всех билдеров в системе.

### Методы

#### setTitle(title: string)
Устанавливает заголовок сущности.

```typescript
entity.setTitle('Новый заголовок');
```

#### setRequired(fieldName: string)
Устанавливает поле как обязательное.

```typescript
entity.setRequired('fieldName');
```

#### enableAudit()
Включает аудит изменений для сущности.

```typescript
entity.enableAudit();
```

#### enableSearch()
Включает поиск по сущности.

```typescript
entity.enableSearch();
```

## AdditionalServiceBuilder

Класс для создания дополнительных сервисов.

### Методы

#### addService(name: string)
Добавляет новый сервис.

```typescript
system.addService('customService')
  .setTitle('Пользовательский сервис')
  .addMethod('process', 'Process data');
```

#### addMethod(name: string, title: string)
Добавляет метод в сервис.

```typescript
service.addMethod('calculate', 'Calculate result');
```

## BaseSavableEntityBuilder

Базовый класс для сущностей, которые могут быть сохранены.

### Методы

#### addScalarField(name: string, type: string)
Добавляет скалярное поле.

```typescript
entity.addScalarField('name', 'string');
```

#### addLinkField(name: string, type: string, title: string)
Добавляет поле-ссылку.

```typescript
entity.addLinkField('customer', 'customerId', 'Клиент');
```

#### setDeletable(deletable: boolean)
Устанавливает возможность удаления.

```typescript
entity.setDeletable(false);
```

## DocumentBuilder

Класс для создания документов.

### Методы

#### addDocument(name: string)
Создает новый документ.

```typescript
system.addDocument('order')
  .setTitle('Заказ')
  .addScalarField('number', 'string');
```

#### addPostingRule(rule: PostingRule)
Добавляет правило проведения.

```typescript
document.addPostingRule({
  register: 'sales',
  debit: 'amount',
  credit: 'amount'
});
```

## RegisterBuilder

Базовый класс для создания регистров.

### Методы

#### addSavingsRegister(name: string)
Создает регистр накоплений.

```typescript
system.addSavingsRegister('sales')
  .setTitle('Продажи')
  .addScalarField('amount', 'number');
```

#### addInfoRegister(name: string)
Создает регистр сведений.

```typescript
system.addInfoRegister('contacts')
  .setTitle('Контакты')
  .addScalarField('phone', 'string');
```

#### addDimension(name: string, type: string)
Добавляет измерение.

```typescript
register.addDimension('product', 'productId');
```

#### addResource(name: string, type: string)
Добавляет ресурс.

```typescript
register.addResource('quantity', 'number');
```

## SystemBuilder

Основной класс для работы с системой.

### Методы

#### addEntity(name: string)
Добавляет новую сущность.

```typescript
system.addEntity('customer')
  .setTitle('Клиент')
  .addScalarField('name', 'string');
```

#### removeEntity(name: string)
Удаляет сущность.

```typescript
system.removeEntity('customer');
```

#### hasEntity(name: string)
Проверяет существование сущности.

```typescript
if (system.hasEntity('customer')) {
  // ...
}
```

## Следующие шаги

- Ознакомьтесь с типами сущностей в разделе [Типы сущностей](12-entity-types.md)
- Изучите лучшие практики в разделе [Лучшие практики](08-best-practices.md)
- При возникновении проблем обратитесь к разделу [Устранение неполадок](09-troubleshooting.md) 