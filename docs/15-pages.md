# Страницы

В этом разделе описана работа со страницами в Runlify.

## Что такое страницы

Страницы - это компоненты пользовательского интерфейса, которые используются для:
- Отображения данных
- Взаимодействия с пользователем
- Навигации по приложению
- Визуализации информации

## Создание страницы

```typescript
system.addPage('dashboard')
  .setTitle('Панель управления')
  .setDescription('Основная страница приложения')
  .addComponent('chart', 'ChartComponent')
  .addComponent('table', 'DataTableComponent');
```

## Компоненты страницы

### Графики
```typescript
// Добавление графика
const chart = page.addComponent('chart', 'ChartComponent')
  .setTitle('График продаж')
  .setType('line')
  .setDataSource('sales');
```

### Таблицы
```typescript
// Добавление таблицы
const table = page.addComponent('table', 'DataTableComponent')
  .setTitle('Список заказов')
  .setDataSource('orders')
  .addColumn('number', 'Номер')
  .addColumn('date', 'Дата')
  .addColumn('amount', 'Сумма');
```

### Виджеты
```typescript
// Добавление виджета
const widget = page.addWidget('summaryWidget')
  .setTitle('Общая статистика')
  .setType('summary')
  .setDataSource('orders')
  .addMetric('totalOrders', 'Всего заказов')
  .addMetric('totalAmount', 'Общая сумма')
  .addMetric('averageOrder', 'Средний чек');
```

## Настройка макета

```typescript
// Настройка макета страницы
page.setLayout({
  type: 'grid',
  columns: 2,
  rows: 2,
  items: [
    { component: 'salesChart', x: 0, y: 0, width: 2, height: 1 },
    { component: 'ordersTable', x: 0, y: 1, width: 1, height: 1 },
    { component: 'summaryWidget', x: 1, y: 1, width: 1, height: 1 }
  ]
});
```

## Настройка доступа

```typescript
// Установка прав доступа
page.setAccess({
  roles: ['admin', 'manager'],
  permissions: ['view', 'edit']
});
```

## Добавление действий

```typescript
// Добавление действий
page.addAction('exportData')
  .setTitle('Экспорт данных')
  .setIcon('download')
  .setHandler(async () => {
    // Логика экспорта
    await exportData();
  });
```

## Интеграция с API

```typescript
// Добавление API эндпоинтов
page.addApiEndpoint('getData')
  .setMethod('GET')
  .setPath('/api/data')
  .setHandler(async (req, res) => {
    // Логика получения данных
    const data = await getData();
    res.json(data);
  });
```

## Локальные ссылки

```typescript
// Добавление локальной ссылки
system.addLocalLink('dashboard')
  .setTitle('Панель управления')
  .setPath('/dashboard')
  .setIcon('dashboard')
  .setOrder(1);
```

## Следующие шаги

- Ознакомьтесь с типами сущностей в разделе [Типы сущностей](12-entity-types.md)
- Изучите классы и методы в разделе [Классы и методы](13-classes.md)
- При возникновении проблем обратитесь к разделу [Устранение неполадок](09-troubleshooting.md) 