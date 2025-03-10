# Графики

В этом разделе описана работа с графиками в Runlify.

## Что такое графики

Графики - это компоненты для визуализации данных. Они используются для:
- Отображения трендов
- Сравнения показателей
- Анализа данных
- Визуализации статистики

## Создание графика

```typescript
const chart = system.addChart('salesChart')
  .setTitle('Динамика продаж')
  .setDescription('График продаж по месяцам')
  .setType('line');
```

## Типы графиков

### Линейный график
```typescript
const lineChart = chart.addLineChart()
  .setTitle('Продажи по месяцам')
  .setDataSource('sales')
  .setXAxis('date')
  .setYAxis('amount')
  .setLegend(true);
```

### Столбчатый график
```typescript
const barChart = chart.addBarChart()
  .setTitle('Продажи по категориям')
  .setDataSource('sales')
  .setXAxis('category')
  .setYAxis('amount')
  .setStacked(true);
```

### Круговая диаграмма
```typescript
const pieChart = chart.addPieChart()
  .setTitle('Распределение продаж')
  .setDataSource('sales')
  .setDimension('category')
  .setMetric('amount')
  .setLegend(true);
```

## Настройка данных

```typescript
// Настройка источника данных
chart.setDataSource('sales')
  .setQuery({
    groupBy: ['date'],
    metrics: ['sum(amount)'],
    filters: [
      { field: 'date', operator: '>=', value: '2024-01-01' }
    ]
  });
```

## Настройка стилей

```typescript
// Настройка внешнего вида
chart.setStyle({
  theme: 'light',
  colors: ['#1976d2', '#dc004e', '#4caf50'],
  grid: true,
  tooltip: true
});
```

## Интерактивность

```typescript
// Добавление интерактивности
chart.setInteractive({
  hover: true,
  click: true,
  zoom: true,
  pan: true
});

// Обработка событий
chart.on('click', (data) => {
  handleChartClick(data);
});

chart.on('hover', (data) => {
  handleChartHover(data);
});
```

## Следующие шаги

- Ознакомьтесь с виджетами в разделе [Виджеты](11-widgets.md)
- Изучите UI компоненты в разделе [UI компоненты](11-ui-components.md)
- При возникновении проблем обратитесь к разделу [Устранение неполадок](09-troubleshooting.md) 