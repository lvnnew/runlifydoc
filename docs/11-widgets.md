# Виджеты

В этом разделе описана работа с виджетами в Runlify.

## Что такое виджеты

Виджеты - это компоненты для отображения агрегированной информации и статистики. Они используются для:
- Отображения ключевых показателей (KPI)
- Показа статистики
- Визуализации метрик
- Отображения сводной информации

## Создание виджета

```typescript
const widget = system.addWidget('salesSummary')
  .setTitle('Статистика продаж')
  .setDescription('Общая информация о продажах')
  .setType('summary');
```

## Типы виджетов

### Виджет с метриками
```typescript
const metricsWidget = widget.addMetricsWidget()
  .setTitle('Ключевые показатели')
  .addMetric('totalSales', 'Общие продажи')
  .addMetric('averageOrder', 'Средний чек')
  .addMetric('totalOrders', 'Количество заказов');
```

### Виджет с графиком
```typescript
const chartWidget = widget.addChartWidget()
  .setTitle('Динамика продаж')
  .setType('line')
  .setDataSource('sales')
  .setDimensions(['date'])
  .setMetrics(['amount']);
```

### Виджет с таблицей
```typescript
const tableWidget = widget.addTableWidget()
  .setTitle('Топ продаж')
  .setDataSource('sales')
  .addColumn('product', 'Товар')
  .addColumn('amount', 'Сумма')
  .setSortable(true);
```

## Настройка обновления

```typescript
// Автоматическое обновление
widget.setAutoRefresh({
  enabled: true,
  interval: 300000 // каждые 5 минут
});

// Ручное обновление
widget.setRefreshable(true);
```

## Настройка стилей

```typescript
// Настройка внешнего вида
widget.setStyle({
  theme: 'light',
  layout: 'grid',
  columns: 2,
  spacing: 'medium'
});
```

## Интеграция с данными

```typescript
// Подключение к источнику данных
widget.setDataSource('sales')
  .setQuery({
    groupBy: ['date'],
    metrics: ['sum(amount)'],
    filters: [
      { field: 'date', operator: '>=', value: '2024-01-01' }
    ]
  });
```

## Следующие шаги

- Ознакомьтесь с UI компонентами в разделе [UI компоненты](11-ui-components.md)
- Изучите графики в разделе [Графики](11-charts.md)
- При возникновении проблем обратитесь к разделу [Устранение неполадок](09-troubleshooting.md) 