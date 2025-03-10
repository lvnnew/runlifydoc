# Структура проекта

Проект, созданный с помощью Runlify, имеет четкую и логичную структуру, которая способствует эффективной разработке. Рассмотрим основные компоненты проекта.

## Общая структура

```
my-project/
├── backend/                # Backend часть приложения
│   ├── src/
│   │   ├── meta/         # Описания сущностей и метаданные
│   │   │   ├── addCatalogs.ts
│   │   │   ├── addMenu.ts
│   │   │   ├── addWscEntities.ts
│   │   │   ├── regenBasedOnMeta.ts
│   │   │   ├── metadata.json
│   │   │   └── options.json
│   │   ├── rest/         # REST API endpoints
│   │   │   ├── healthRouter.ts
│   │   │   ├── restRouter.ts
│   │   │   └── getMetricsHandler.ts
│   │   ├── generated/    # Сгенерированный код
│   │   ├── graph/        # GraphQL схемы и резолверы
│   │   ├── workers/      # Фоновые задачи
│   │   ├── utils/        # Утилиты
│   │   ├── types/        # TypeScript типы
│   │   ├── systemHooks/  # Системные хуки
│   │   ├── metrics/      # Метрики и мониторинг
│   │   ├── init/         # Инициализация
│   │   ├── integrationClients/ # Клиенты для интеграций
│   │   ├── decorators/   # TypeScript декораторы
│   │   ├── clients/      # API клиенты
│   │   ├── cli/          # Командная строка
│   │   ├── businessTests/# Бизнес-тесты
│   │   ├── app/          # Основной код приложения
│   │   ├── bots/         # Боты и автоматизация
│   │   ├── adm/          # Административные функции
│   │   └── config/       # Конфигурация приложения
│   ├── prisma/           # Схемы и миграции базы данных
│   ├── compose/          # Docker конфигурация
│   ├── chart/            # Helm чарты для Kubernetes
│   ├── runlify.json      # Конфигурация Runlify
│   └── package.json      # Зависимости backend
│
└── frontend/             # Frontend часть приложения
    ├── src/
    │   ├── adm/          # Административная панель
    │   │   ├── widgets/  # Виджеты и компоненты
    │   │   ├── utility/  # Утилитарные функции
    │   │   ├── pages/    # Страницы
    │   │   ├── functions/# Бизнес-логика
    │   │   ├── commonActions/ # Общие действия
    │   │   ├── resourcesChunk0.tsx # React Admin ресурсы (часть 1)
    │   │   ├── resourcesChunk1.tsx # React Admin ресурсы (часть 2)
    │   │   ├── routes.tsx # Маршрутизация
    │   │   ├── resources.tsx # Описание ресурсов
    │   │   ├── getAdditionalMenu.ts # Дополнительное меню
    │   │   ├── getDefaultMenu.ts # Основное меню
    │   │   ├── entityMapping.ts # Маппинг сущностей
    │   │   ├── additionalRoutes.tsx # Дополнительные маршруты
    │   │   ├── MetaPage.tsx # Страница метаданных
    │   │   ├── ResourcesPage.tsx # Страница ресурсов
    │   │   ├── Dashboard.tsx # Дашборд
    │   │   └── DashboardStats.tsx # Статистика
    │   ├── components/   # Общие компоненты
    │   ├── generated/    # Сгенерированный код
    │   ├── hooks/        # React хуки
    │   ├── layouts/      # Шаблоны страниц
    │   ├── pages/        # Страницы приложения
    │   ├── services/     # Сервисы
    │   ├── store/        # Redux store
    │   ├── styles/       # Стили
    │   ├── types/        # TypeScript типы
    │   └── utils/        # Утилиты
    ├── public/           # Статические файлы
    ├── config/           # Конфигурация
    └── package.json      # Зависимости frontend
```

## Backend структура

### Директория src/meta

Это ключевая директория, где хранятся описания сущностей и метаданные приложения:

- `addCatalogs.ts` - Описания основных сущностей (каталогов)
- `addMenu.ts` - Конфигурация навигационного меню
- `addWscEntities.ts` - Дополнительные сущности
- `regenBasedOnMeta.ts` - Скрипт регенерации кода
- `metadata.json` - Сгенерированные метаданные
- `options.json` - Настройки генерации

### Директория src/rest

REST API endpoints и middleware:

- `healthRouter.ts` - Проверка здоровья сервиса
- `restRouter.ts` - Основной роутер API
- `getMetricsHandler.ts` - Обработчик метрик

### Директория src/generated

Содержит автоматически сгенерированный код:

- API endpoints
- Типы TypeScript
- Валидаторы
- Маппинги данных

### Директория prisma

Управление базой данных:

- Схема базы данных
- Миграции
- Сиды (начальные данные)

## Frontend структура

### Директория src/adm

Административная панель на базе React Admin:

- `widgets/` - Переиспользуемые компоненты и виджеты
- `utility/` - Вспомогательные функции
- `pages/` - Страницы админки
- `functions/` - Бизнес-логика
- `commonActions/` - Общие действия
- `resourcesChunk0.tsx`, `resourcesChunk1.tsx` - React Admin ресурсы
- `routes.tsx` - Конфигурация маршрутов
- `resources.tsx` - Описание доступных ресурсов
- `getAdditionalMenu.ts`, `getDefaultMenu.ts` - Конфигурация меню
- `entityMapping.ts` - Маппинг сущностей на компоненты
- `MetaPage.tsx`, `ResourcesPage.tsx` - Системные страницы
- `Dashboard.tsx`, `DashboardStats.tsx` - Дашборд и статистика

### Директория src/components

Общие компоненты:

- Базовые UI элементы
- Утилитарные компоненты
- Переиспользуемые модули

### Директория src/generated

Автоматически сгенерированный код:

- API клиенты
- Типы TypeScript
- Компоненты форм
- Компоненты таблиц

## Конфигурационные файлы

### runlify.json

Основной конфигурационный файл Runlify:

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "database": {
    "type": "postgresql",
    "url": "postgresql://user:password@localhost:5432/db"
  },
  "generation": {
    "output": "./src/generated",
    "language": "typescript"
  }
}
```

### runlify.developer.json

Локальные настройки разработчика (не включается в репозиторий):

```json
{
  "database": {
    "url": "postgresql://user:password@localhost:5432/db_dev"
  }
}
```

## Рекомендации по организации

1. **Разделение кода**
   - Держите пользовательский код отдельно от сгенерированного
   - Используйте подпапки для группировки связанных компонентов
   - Следуйте принципу единой ответственности

2. **Именование**
   - Используйте понятные и описательные имена
   - Следуйте принятым конвенциям именования
   - Группируйте связанные файлы в директории

3. **Версионный контроль**
   - Не включайте сгенерированные файлы в git
   - Храните конфигурации в репозитории
   - Используйте .gitignore для исключения временных файлов

4. **Документация**
   - Документируйте пользовательский код
   - Описывайте нестандартные решения
   - Ведите changelog

## Дополнительные скрипты

В проекте доступны различные скрипты для автоматизации:

```bash
# Инициализация проекта
yarn init:base

# Инициализация прав доступа
yarn init:permissions

# Миграции базы данных
yarn prisma:newMigration

# Генерация кода
yarn regen

# Проверка кода
yarn lint
```

## Следующие шаги

- Изучите [руководство по кодогенерации](./05-code-generation-guide.md)
- Познакомьтесь с [API Reference](./06-api-reference.md)
- Посмотрите [примеры использования](./07-examples.md)
- Изучите [детальную структуру фреймворка Runlify](./10-runlify-structure.md) 