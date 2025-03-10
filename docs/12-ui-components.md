# UI компоненты

В этом разделе описана работа с UI компонентами в Runlify.

## Что такое UI компоненты

UI компоненты - это переиспользуемые элементы интерфейса, которые используются для:
- Создания форм
- Отображения данных
- Взаимодействия с пользователем
- Навигации

## Создание компонента

```typescript
const component = system.addComponent('customForm')
  .setTitle('Пользовательская форма')
  .setDescription('Форма для ввода данных')
  .setType('form');
```

## Типы компонентов

### Формы
```typescript
const form = component.addForm()
  .setTitle('Регистрация')
  .addField('name', 'text', 'Имя')
  .addField('email', 'email', 'Email')
  .addField('password', 'password', 'Пароль')
  .setSubmitHandler(async (data) => {
    await registerUser(data);
  });
```

### Таблицы
```typescript
const table = component.addTable()
  .setTitle('Список пользователей')
  .addColumn('name', 'Имя')
  .addColumn('email', 'Email')
  .addColumn('role', 'Роль')
  .setSortable(true)
  .setFilterable(true);
```

### Навигация
```typescript
const navigation = component.addNavigation()
  .setTitle('Меню')
  .addItem('home', 'Главная', '/')
  .addItem('profile', 'Профиль', '/profile')
  .addItem('settings', 'Настройки', '/settings');
```

## Стилизация

```typescript
// Настройка темы
component.setTheme({
  primary: '#1976d2',
  secondary: '#dc004e',
  background: '#ffffff'
});

// Настройка отступов
component.setSpacing({
  padding: '16px',
  margin: '8px'
});
```

## Валидация

```typescript
// Добавление валидации
form.addValidation('email', {
  required: true,
  pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
  message: 'Введите корректный email'
});
```

## Обработка событий

```typescript
// Добавление обработчиков
component.on('submit', async (data) => {
  await handleSubmit(data);
});

component.on('change', (field, value) => {
  handleFieldChange(field, value);
});
```

## Следующие шаги

- Ознакомьтесь с виджетами в разделе [Виджеты](11-widgets.md)
- Изучите графики в разделе [Графики](11-charts.md)
- При возникновении проблем обратитесь к разделу [Устранение неполадок](09-troubleshooting.md) 