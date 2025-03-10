# Воркеры и страницы

В этом разделе описана работа с воркерами и страницами в Runlify.

## Воркеры

Воркеры - это фоновые процессы, которые выполняют длительные задачи.

### Создание воркера

```typescript
system.addWorker('dataProcessor')
  .setTitle('Обработчик данных')
  .setDescription('Обрабатывает данные в фоновом режиме')
  .addMethod('process', 'Обработка данных');
```

### Методы воркера

```typescript
// Метод обработки
async function process(data: any) {
  // Логика обработки
  await processData(data);
}

// Метод остановки
async function stop() {
  // Логика остановки
  await cleanup();
}
```

### Конфигурация ресурсов

```typescript
// Установка ресурсов
worker.setResources({
  cpu: 1,
  memory: '1Gi',
  storage: '10Gi'
});
```

## Страницы

Страницы - это компоненты пользовательского интерфейса.

### Создание страницы

```typescript
system.addPage('dashboard')
  .setTitle('Панель управления')
  .setDescription('Основная страница приложения')
  .addComponent('chart', 'ChartComponent')
  .addComponent('table', 'DataTableComponent');
```

### Компоненты страницы

```typescript
// График
const chart = page.addComponent('chart', 'ChartComponent')
  .setTitle('График продаж')
  .setType('line')
  .setDataSource('sales');

// Таблица
const table = page.addComponent('table', 'DataTableComponent')
  .setTitle('Список заказов')
  .setDataSource('orders')
  .addColumn('number', 'Номер')
  .addColumn('date', 'Дата')
  .addColumn('amount', 'Сумма');
```

### Настройка доступа

```typescript
// Установка прав доступа
page.setAccess({
  roles: ['admin', 'manager'],
  permissions: ['view', 'edit']
});
```

## Добавление элементов в контекст

### Системные параметры

```typescript
// Добавление системного параметра
system.addParameter('apiUrl')
  .setTitle('URL API')
  .setDescription('Базовый URL для API')
  .setDefault('http://localhost:3000')
  .setRequired(true);
```

### Локальные ссылки

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