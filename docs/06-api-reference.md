# API Reference

## Основные классы

### SystemMetaBuilder

Основной класс для описания системы. Позволяет добавлять каталоги, документы, информационные регистры и другие сущности.

#### Методы

- `addCatalog(name: string)`: Добавляет новый каталог
- `addDocument(name: string)`: Добавляет новый документ
- `addInfoRegistry(name: string)`: Добавляет новый информационный регистр
- `addSumRegistry(name: string)`: Добавляет новый регистр накопления
- `addRole(name: string)`: Добавляет новую роль
- `addPermission(name: string)`: Добавляет новое разрешение
- `addPage(name: string)`: Добавляет новую страницу
- `addRestApi(name: string)`: Добавляет новое REST API
- `addTelegramBot(name: string)`: Добавляет нового Telegram бота
- `addDeployment(name: string)`: Добавляет новую конфигурацию развертывания
- `addConfigVar(name: string)`: Добавляет новую конфигурационную переменную
- `addAdditionalService(name: string)`: Добавляет новый дополнительный сервис

### BaseBuilder

Базовый класс для всех билдеров. Предоставляет общие методы.

#### Методы

- `setName(name: string)`: Устанавливает имя
- `setTitle(title: string)`: Устанавливает заголовок
- `setDescription(description: string)`: Устанавливает описание
- `setIcon(icon: string)`: Устанавливает иконку

### BaseSavableEntityBuilder

Базовый класс для сущностей с возможностью сохранения (каталоги, документы, регистры).

#### Методы

- `addScalarField(name: string, type: ScalarType)`: Добавляет скалярное поле
- `addLinkField(name: string, target: string)`: Добавляет поле-ссылку
- `addViewLinkField(name: string, target: string)`: Добавляет поле для отображения связи
- `addIdField(name: string)`: Добавляет поле идентификатора
- `addModelField(name: string)`: Добавляет поле модели
- `setTitleField(fieldName: string)`: Устанавливает поле заголовка
- `setRequired(fieldName: string)`: Делает поле обязательным
- `setUnique(fieldName: string)`: Делает поле уникальным
- `setSearchable(fieldName: string)`: Делает поле доступным для поиска
- `setNotUpdatableByUser(fieldName: string)`: Запрещает обновление поля пользователем
- `addMethod(name: string)`: Добавляет пользовательский метод
- `enableAudit()`: Включает аудит изменений
- `enableSearch()`: Включает поиск
- `enableFilter()`: Включает фильтрацию
- `setPermissions(permissions: PermissionType[])`: Устанавливает разрешения

### CatalogBuilder

Класс для создания каталогов.

#### Методы

Наследует все методы от BaseSavableEntityBuilder и добавляет:

- `setHierarchical()`: Делает каталог иерархическим
- `setOwnerField(fieldName: string)`: Устанавливает поле владельца

### DocumentBuilder

Класс для создания документов.

#### Методы

Наследует все методы от BaseSavableEntityBuilder и добавляет:

- `setNumberPrefix(prefix: string)`: Устанавливает префикс номера
- `enablePosting()`: Включает проведение
- `addMovement(registryName: string)`: Добавляет движение по регистру

### InfoRegistryBuilder и SumRegistryBuilder

Классы для создания регистров.

#### Методы

Наследуют все методы от BaseSavableEntityBuilder и добавляют:

- `addDimension(name: string)`: Добавляет измерение
- `addResource(name: string)`: Добавляет ресурс

## Типы полей

### ScalarFieldBuilder

Класс для создания скалярных полей.

#### Доступные типы

- `string`: Строка
- `int`: Целое число
- `float`: Число с плавающей точкой
- `bool`: Логическое значение
- `date`: Дата
- `datetime`: Дата и время
- `time`: Время
- `json`: JSON
- `file`: Файл
- `image`: Изображение

### LinkFieldBuilder

Класс для создания полей-ссылок.

#### Типы связей

- `oneToOne`: Один к одному
- `oneToMany`: Один ко многим
- `manyToMany`: Многие ко многим

## Дополнительные классы

### RestApiBuilder и RestApiMethodBuilder

Классы для создания REST API и методов.

#### Методы

- `addMethod(name: string, type: MethodType)`: Добавляет метод
- `setPath(path: string)`: Устанавливает путь
- `setRequestSchema(schema: object)`: Устанавливает схему запроса
- `setResponseSchema(schema: object)`: Устанавливает схему ответа

### PageBuilder

Класс для создания страниц.

#### Методы

- `setLayout(layout: string)`: Устанавливает макет
- `addWidget(widget: WidgetType)`: Добавляет виджет
- `setPermissions(permissions: string[])`: Устанавливает разрешения

### TelegramBotBuilder

Класс для создания Telegram ботов.

#### Методы

- `setToken(token: string)`: Устанавливает токен
- `addCommand(command: string)`: Добавляет команду
- `setWebhook(url: string)`: Устанавливает вебхук

### DeploymentBuilder

Класс для настройки развертывания.

#### Методы

- `setEnvironment(env: string)`: Устанавливает окружение
- `addVariable(name: string, value: string)`: Добавляет переменную
- `setDocker(config: object)`: Настраивает Docker

## Типы и перечисления

### MethodType

Типы методов для сервисов:
- `create`
- `update`
- `delete`
- `get`
- `list`

### PermissionType

Типы разрешений:
- `create`
- `read`
- `update`
- `delete`
- `list`

### WidgetType

Типы виджетов:
- `list`
- `form`
- `chart`
- `dashboard`

## Примеры использования

### Создание каталога пользователей

```typescript
const users = meta.addCatalog('users')
  .setTitle('Пользователи')
  .addScalarField('email', 'string')
  .setRequired('email')
  .setUnique('email')
  .addScalarField('name', 'string')
  .setTitleField('name')
  .enableSearch()
  .enableAudit();
```

### Создание документа заказа

```typescript
const orders = meta.addDocument('orders')
  .setTitle('Заказы')
  .setNumberPrefix('ORD')
  .addLinkField('user', 'users')
  .setRequired('user')
  .addScalarField('amount', 'float')
  .setRequired('amount')
  .enablePosting()
  .addMovement('balance');
```

## Следующие шаги

- Изучите примеры использования в разделе [Примеры](07-examples.md)
- Ознакомьтесь с лучшими практиками в разделе [Лучшие практики](08-best-practices.md)
- При возникновении проблем обратитесь к разделу [Устранение неполадок](09-troubleshooting.md)

## Команды для разработки и развертывания

### Docker и окружение

- `compose:start`: Запускает Docker-контейнеры проекта
  ```bash
  docker compose -f compose/docker-compose.yml -p sport up starter
  ```

- `compose:stop`: Останавливает Docker-контейнеры
  ```bash
  docker compose -f compose/docker-compose.yml -p sport stop
  ```

- `compose:delete`: Удаляет Docker-контейнеры и их тома
  ```bash
  docker compose -f compose/docker-compose.yml -p sport down --volumes
  ```

### Разработка

- `build`: Компилирует TypeScript код
  ```bash
  (rm -rf dist || true) && tsc
  ```

- `start`: Запускает приложение в production режиме
  ```bash
  runlify start env=prod node --unhandled-rejections=strict dist/index.js
  ```

- `dev`: Запускает приложение в режиме разработки с автоперезагрузкой
  ```bash
  ts-node-dev --files src/index.ts
  ```

- `dev:dev`: Запускает в stage окружении
  ```bash
  runlify start env=stage yarn dev
  ```

- `dev:local`: Запускает в локальном окружении
  ```bash
  runlify start env=local yarn dev
  ```

- `dev:prod`: Запускает в production окружении
  ```bash
  runlify start env=prod yarn dev
  ```

### База данных и Prisma

- `prisma:gen`: Генерирует Prisma клиент
  ```bash
  prisma generate
  ```

- `prisma:newMigration`: Создает новую миграцию базы данных
  ```bash
  runlify start env=migration prisma migrate dev --preview-feature
  ```

- `prisma:deploy`: Применяет миграции на production
  ```bash
  prisma migrate deploy --schema prisma/deployConnection.prisma
  ```

### Инициализация и генерация кода

- `init:base`: Выполняет базовую инициализацию проекта
  ```bash
  ts-node src/init/baseInit.ts
  ```

- `init:permissions`: Инициализирует роли и разрешения
  ```bash
  yarn ts-node:withContext src/init/roles/initRolesWithPermissions.ts
  ```

- `init:dev`: Инициализирует окружение разработки
  ```bash
  ts-node src/init/initDev.ts
  ```

- `regen`: Регенерирует код на основе метаданных
  ```bash
  yarn ts-node src/meta/regenBasedOnMeta.ts && runlify regen
  ```

### Тестирование и проверка кода

- `test`: Запускает тесты
  ```bash
  jest --maxWorkers 2
  ```

- `lint`: Проверяет код на соответствие стандартам
  ```bash
  eslint src
  ```

- `typecheck`: Проверяет типы TypeScript
  ```bash
  tsc -P tsconfig.json
  ```

- `typecheck:watch`: Проверяет типы в режиме наблюдения
  ```bash
  tsc -P tsconfig.json --watch
  ```

### Дополнительные команды

- `gen`: Генерирует GraphQL схемы
  ```bash
  yarn ts-node src/gen/genGQSchemes.ts
  ```

- `serve`: Запускает статический сервер для публичных файлов
  ```bash
  serve public
  ``` 