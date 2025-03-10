# Быстрый старт

Это руководство поможет вам быстро начать работу с Runlify и создать ваш первый проект.

## Минимальные требования

Перед началом работы убедитесь, что у вас установлено:

- Node.js (версия 16 или выше)
- npm или yarn
- Git
- TypeScript
- Редактор кода (рекомендуется VS Code)

## Установка Runlify

1. Установите Runlify глобально через npm:

```bash
npm install -g runlify
```

или через yarn:

```bash
yarn global add runlify
```

2. Проверьте установку:

```bash
runlify --version
```

## Создание первого проекта

1. Создайте новый проект:

```bash
runlify create my-first-project
cd my-first-project
```

2. Установите зависимости:

```bash
yarn install
```

3. Создайте первую сущность. Создайте файл `src/entities/User.ts`:

```typescript
import { CatalogBuilder } from 'runlify';

export const User = new CatalogBuilder('user')
  .setTitle({ singular: 'Пользователь', plural: 'Пользователи' })
  .setMaterialUiIcon('Person')
  .addField('name', 'Имя')
  .addField('email', 'Email')
  .addField('age', 'Возраст')
  .setSearchEnabled(true)
  .setAuditable(true);
```

4. Сгенерируйте код:

```bash
runlify generate
```

5. Запустите приложение:

```bash
# Запуск backend
yarn start:backend

# В новом терминале запустите frontend
yarn start:frontend
```

После запуска, ваше приложение будет доступно:
- Frontend: http://localhost:3000
- Backend API: http://localhost:4000

## Что дальше?

Теперь, когда у вас есть работающее приложение, вы можете:

1. Изучить [структуру проекта](./04-project-structure.md)
2. Узнать больше о [работе с сущностями](./05-code-generation-guide.md)
3. Познакомиться с [API Reference](./06-api-reference.md)

## Основные команды

```bash
# Создать новый проект
runlify create <project-name>

# Сгенерировать код
runlify generate

# Запустить миграции
runlify migrate

# Откатить последнюю миграцию
runlify migrate:revert

# Показать статус миграций
runlify migrate:status
```

## Типичные проблемы

Если вы столкнулись с проблемами при установке или запуске, проверьте:

1. Версии Node.js и npm/yarn
2. Наличие всех необходимых зависимостей
3. Правильность конфигурации в `runlify.config.ts`
4. Логи в консоли на наличие ошибок

Более подробную информацию о решении проблем можно найти в разделе [Troubleshooting](./09-troubleshooting.md). 