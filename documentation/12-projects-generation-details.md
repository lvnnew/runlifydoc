# Подробная документация по генерации проектов

## Обзор модуля projectsGeneration

Модуль `projectsGeneration` является ключевым компонентом Runlify CLI, отвечающим за генерацию проектов, сущностей, окружений и других элементов. Этот модуль предоставляет мощные инструменты для создания структурированных проектов с предварительно настроенной архитектурой, следуя лучшим практикам разработки.

## Структура модуля

Модуль `projectsGeneration` имеет следующую структуру:

```
src/projectsGeneration/
├── builders/              # Строители для различных типов сущностей
├── commonEntities/        # Общие сущности, используемые в проектах
├── defaultCatalogs/       # Шаблоны каталогов по умолчанию
├── fileCleaners/          # Утилиты для очистки сгенерированных файлов
├── generators/            # Генераторы для различных частей проекта
│   ├── fileTemplates/     # Шаблоны файлов
│   │   ├── back/          # Шаблоны для бэкенда
│   │   ├── ui/            # Шаблоны для пользовательского интерфейса
│   │   └── prisma/        # Шаблоны для Prisma
│   └── graph/             # Генераторы для GraphQL
├── links/                 # Утилиты для работы со связями между сущностями
├── modules/               # Модули для расширения функциональности
├── args.ts                # Аргументы для генерации
├── dataForTests.ts        # Данные для тестирования
├── generateAdditionalService.ts # Генерация дополнительных сервисов
├── generateEntity.ts      # Генерация сущностей
├── generateEnvironment.ts # Генерация окружений
├── generateProject.ts     # Основной файл для генерации проектов
├── genGraphSchemesByLocalGenerator.ts # Генерация GraphQL схем
├── index.ts               # Экспорты модуля
├── metaUtils.ts           # Утилиты для работы с метаданными
├── types.ts               # Типы и интерфейсы
└── utils.ts               # Вспомогательные функции
```

## Основные компоненты

### Builders (Строители)

Строители предоставляют API для создания различных типов сущностей:

- **BaseBuilder** - Базовый строитель для всех типов сущностей
- **CatalogBuilder** - Строитель для каталогов (основных сущностей)
- **ReportBuilder** - Строитель для отчетов
- **InfoRegistryBuilder** - Строитель для информационных регистров
- **SumRegistryBuilder** - Строитель для регистров накопления

### Generators (Генераторы)

Генераторы отвечают за создание различных частей проекта:

- **fileTemplates** - Шаблоны файлов для различных частей проекта
  - **back** - Шаблоны для бэкенда (контроллеры, сервисы, модели)
  - **ui** - Шаблоны для пользовательского интерфейса (компоненты, страницы)
  - **prisma** - Шаблоны для Prisma (схемы, миграции)
- **graph** - Генераторы для GraphQL (схемы, резолверы)

### Основные функции

Модуль `projectsGeneration` экспортирует следующие основные функции:

- **generateProject** - Генерация полного проекта
- **generateEntity** - Генерация отдельной сущности
- **generateEnvironment** - Генерация окружения
- **genGraphSchemesByLocalGenerator** - Генерация GraphQL схем

## Использование модуля projectsGeneration

### Генерация полного проекта

Для генерации полного проекта используется функция `generateProject`:

```typescript
import { generateProject, System } from 'runlify/projectsGeneration';

// Создание системы
const system: System = {
  name: 'MyProject',
  description: 'My Awesome Project',
  entities: [],
  // Другие параметры системы
};

// Опции генерации
const options = {
  genPrismaServices: true,
  genGraphSchema: true,
  genGraphResolvers: true,
  genUiResources: true,
  skipWarningThisIsGenerated: false,
  genPrismaSchema: true,
  genContext: true,
  typesOnly: false,
  // Другие опции
};

// Генерация проекта
await generateProject(system, options);
```

### Создание и генерация сущности

Для создания и генерации сущности используются строители и функция `generateEntity`:

```typescript
import { CatalogBuilder, generateEntity } from 'runlify/projectsGeneration';

// Создание сущности с помощью строителя
const userEntity = new CatalogBuilder('User')
  .addTextField('name', { required: true })
  .addTextField('email', { required: true, unique: true })
  .addPasswordField('password', { required: true })
  .addDateField('birthDate')
  .addBooleanField('isActive', { defaultValue: true })
  .build();

// Опции генерации
const options = {
  genPrismaServices: true,
  genGraphSchema: true,
  genGraphResolvers: true,
  genUiResources: true,
  // Другие опции
};

// Генерация сущности
await generateEntity(userEntity, options);
```

### Создание связей между сущностями

Для создания связей между сущностями используются методы строителей:

```typescript
import { CatalogBuilder } from 'runlify/projectsGeneration';

// Создание сущности Post
const postEntity = new CatalogBuilder('Post')
  .addTextField('title', { required: true })
  .addTextField('content', { required: true })
  .addDateField('publishedAt')
  .build();

// Создание сущности User со связью к Post
const userEntity = new CatalogBuilder('User')
  .addTextField('name', { required: true })
  .addTextField('email', { required: true, unique: true })
  // Связь "один ко многим" с Post
  .addLinkToMany('posts', 'Post', {
    inverseSide: 'author',
    required: false,
  })
  .build();

// Создание сущности Category со связью "многие ко многим" с Post
const categoryEntity = new CatalogBuilder('Category')
  .addTextField('name', { required: true })
  // Связь "многие ко многим" с Post
  .addLinkToMany('posts', 'Post', {
    inverseSide: 'categories',
    required: false,
    manyToMany: true,
  })
  .build();
```

### Генерация окружения

Для генерации окружения используется функция `generateEnvironment`:

```typescript
import { generateEnvironment } from 'runlify/projectsGeneration';

// Параметры окружения
const environmentParams = {
  name: 'development',
  databaseUrl: 'postgresql://user:password@localhost:5432/mydb',
  jwtSecret: 'my-secret-key',
  port: 4000,
  // Другие параметры
};

// Генерация окружения
await generateEnvironment(environmentParams);
```

## Примеры использования

### Пример 1: Создание проекта для управления задачами

```typescript
import { 
  CatalogBuilder, 
  generateProject, 
  System 
} from 'runlify/projectsGeneration';

// Создание сущности Task
const taskEntity = new CatalogBuilder('Task')
  .addTextField('title', { required: true })
  .addTextField('description', { required: false })
  .addEnumField('status', ['TODO', 'IN_PROGRESS', 'DONE'], { defaultValue: 'TODO' })
  .addDateField('dueDate', { required: false })
  .addBooleanField('isCompleted', { defaultValue: false })
  .build();

// Создание сущности User
const userEntity = new CatalogBuilder('User')
  .addTextField('name', { required: true })
  .addTextField('email', { required: true, unique: true })
  .addPasswordField('password', { required: true })
  // Связь "один ко многим" с Task
  .addLinkToMany('tasks', 'Task', {
    inverseSide: 'assignee',
    required: false,
  })
  .build();

// Создание сущности Project
const projectEntity = new CatalogBuilder('Project')
  .addTextField('name', { required: true })
  .addTextField('description', { required: false })
  // Связь "один ко многим" с Task
  .addLinkToMany('tasks', 'Task', {
    inverseSide: 'project',
    required: false,
  })
  // Связь "многие ко многим" с User
  .addLinkToMany('members', 'User', {
    inverseSide: 'projects',
    required: false,
    manyToMany: true,
  })
  .build();

// Создание системы
const system: System = {
  name: 'TaskManager',
  description: 'Task Management System',
  entities: [taskEntity, userEntity, projectEntity],
};

// Генерация проекта
await generateProject(system);
```

### Пример 2: Создание проекта для электронной коммерции

```typescript
import { 
  CatalogBuilder, 
  generateProject, 
  System 
} from 'runlify/projectsGeneration';

// Создание сущности Product
const productEntity = new CatalogBuilder('Product')
  .addTextField('name', { required: true })
  .addTextField('description', { required: false })
  .addNumberField('price', { required: true, min: 0 })
  .addNumberField('stock', { required: true, min: 0, defaultValue: 0 })
  .addBooleanField('isAvailable', { defaultValue: true })
  .addImageField('image', { required: false })
  .build();

// Создание сущности Category
const categoryEntity = new CatalogBuilder('Category')
  .addTextField('name', { required: true })
  .addTextField('description', { required: false })
  // Связь "многие ко многим" с Product
  .addLinkToMany('products', 'Product', {
    inverseSide: 'categories',
    required: false,
    manyToMany: true,
  })
  .build();

// Создание сущности Customer
const customerEntity = new CatalogBuilder('Customer')
  .addTextField('firstName', { required: true })
  .addTextField('lastName', { required: true })
  .addTextField('email', { required: true, unique: true })
  .addTextField('phone', { required: false })
  .addPasswordField('password', { required: true })
  .build();

// Создание сущности Order
const orderEntity = new CatalogBuilder('Order')
  .addDateField('orderDate', { required: true, defaultValue: 'now()' })
  .addEnumField('status', ['PENDING', 'PROCESSING', 'SHIPPED', 'DELIVERED', 'CANCELLED'], { defaultValue: 'PENDING' })
  .addNumberField('totalAmount', { required: true, min: 0 })
  // Связь "многие к одному" с Customer
  .addLinkToOne('customer', 'Customer', { required: true })
  .build();

// Создание сущности OrderItem
const orderItemEntity = new CatalogBuilder('OrderItem')
  .addNumberField('quantity', { required: true, min: 1, defaultValue: 1 })
  .addNumberField('price', { required: true, min: 0 })
  // Связь "многие к одному" с Order
  .addLinkToOne('order', 'Order', { required: true })
  // Связь "многие к одному" с Product
  .addLinkToOne('product', 'Product', { required: true })
  .build();

// Создание системы
const system: System = {
  name: 'ECommerce',
  description: 'E-Commerce System',
  entities: [productEntity, categoryEntity, customerEntity, orderEntity, orderItemEntity],
};

// Генерация проекта
await generateProject(system);
```

## Расширенные возможности

### Настройка генерации UI

Вы можете настроить генерацию пользовательского интерфейса для сущностей:

```typescript
import { CatalogBuilder } from 'runlify/projectsGeneration';

const userEntity = new CatalogBuilder('User')
  .addTextField('name', { required: true })
  .addTextField('email', { required: true, unique: true })
  // Настройка отображения в UI
  .setUiOptions({
    list: {
      gen: true,
      fields: ['name', 'email', 'createdAt'],
    },
    show: {
      gen: true,
      fields: ['name', 'email', 'createdAt', 'updatedAt'],
    },
    edit: {
      gen: true,
      fields: ['name', 'email'],
      idEditable: false,
    },
    create: {
      gen: true,
      fields: ['name', 'email', 'password'],
    },
    menu: {
      show: true,
      icon: 'UserIcon',
      group: 'Administration',
    },
    resourcesPage: {
      show: true,
    },
  })
  .build();
```

### Настройка валидации

Вы можете настроить валидацию полей сущностей:

```typescript
import { CatalogBuilder } from 'runlify/projectsGeneration';

const productEntity = new CatalogBuilder('Product')
  .addTextField('name', { 
    required: true,
    minLength: 3,
    maxLength: 100,
    validate: {
      pattern: '^[a-zA-Z0-9 ]+$',
      message: 'Name can only contain alphanumeric characters and spaces',
    },
  })
  .addNumberField('price', { 
    required: true,
    min: 0.01,
    max: 9999.99,
    validate: {
      custom: 'value => value % 0.01 === 0',
      message: 'Price must have at most 2 decimal places',
    },
  })
  .build();
```

### Создание пользовательских типов полей

Вы можете создавать пользовательские типы полей для сущностей:

```typescript
import { CatalogBuilder } from 'runlify/projectsGeneration';

// Расширение строителя для добавления пользовательского типа поля
CatalogBuilder.prototype.addGeoPointField = function(name, options = {}) {
  return this.addField(name, {
    type: 'object',
    properties: {
      latitude: { type: 'number', minimum: -90, maximum: 90 },
      longitude: { type: 'number', minimum: -180, maximum: 180 },
    },
    required: ['latitude', 'longitude'],
    ...options,
  });
};

// Использование пользовательского типа поля
const locationEntity = new CatalogBuilder('Location')
  .addTextField('name', { required: true })
  .addGeoPointField('coordinates', { required: true })
  .build();
```

## Рекомендации по использованию

- **Планируйте структуру** - Перед генерацией проекта тщательно планируйте структуру сущностей и их связей
- **Используйте строители** - Используйте строители для создания сущностей, это обеспечит правильную структуру и валидацию
- **Настраивайте UI** - Настраивайте пользовательский интерфейс для каждой сущности в соответствии с потребностями пользователей
- **Добавляйте валидацию** - Добавляйте валидацию полей для обеспечения целостности данных
- **Тестируйте генерацию** - Тестируйте генерацию проекта на небольших примерах перед созданием полного проекта
- **Используйте версионирование** - Храните определения сущностей в системе контроля версий для отслеживания изменений

## Заключение

Модуль `projectsGeneration` предоставляет мощные инструменты для генерации проектов, сущностей и окружений. Используя этот модуль, вы можете быстро создавать структурированные проекты с предварительно настроенной архитектурой, следуя лучшим практикам разработки. Это значительно ускоряет процесс разработки и обеспечивает согласованность кодовой базы. 