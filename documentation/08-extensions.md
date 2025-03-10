# Расширения

## Обзор

Runlify CLI поддерживает систему расширений, которая позволяет добавлять новые функциональные возможности, команды и интеграции. Расширения могут быть разработаны как самой командой Runlify, так и сообществом или вашей собственной командой. В этом разделе описаны различные аспекты работы с расширениями в Runlify CLI.

## Типы расширений

Runlify CLI поддерживает следующие типы расширений:

- **Команды** - Добавляют новые команды в CLI
- **Генераторы** - Добавляют новые шаблоны для генерации кода
- **Интеграции** - Обеспечивают интеграцию с внешними сервисами и API
- **Хуки** - Позволяют выполнять код на различных этапах жизненного цикла проекта
- **Утилиты** - Добавляют вспомогательные функции и инструменты

## Установка расширений

Для установки расширения используйте команду `runlify extension install` (или сокращенно `runlify ext install`):

```bash
runlify extension install <name>
runlify ext install <name>
```

Вы также можете установить расширение из локального файла или Git-репозитория:

```bash
# Установка из локального файла
runlify ext install ./path/to/extension

# Установка из Git-репозитория
runlify ext install github:username/repo

# Установка конкретной версии
runlify ext install extension-name@1.2.3
```

## Управление расширениями

Для управления установленными расширениями используйте следующие команды:

```bash
# Список установленных расширений
runlify ext list

# Обновление расширения
runlify ext update <name>

# Удаление расширения
runlify ext remove <name>

# Информация о расширении
runlify ext info <name>
```

## Конфигурация расширений

Конфигурация расширений хранится в файле `runlify.json` в корневой директории проекта:

```json
{
  "extensions": {
    "runlify-ext-docker": {
      "enabled": true,
      "config": {
        "registry": "docker.io",
        "username": "user",
        "buildArgs": {
          "NODE_ENV": "production"
        }
      }
    },
    "runlify-ext-aws": {
      "enabled": true,
      "config": {
        "region": "us-west-2",
        "profile": "default"
      }
    }
  }
}
```

## Официальные расширения

Runlify CLI предоставляет несколько официальных расширений:

### runlify-ext-docker

Расширение для работы с Docker.

```bash
runlify ext install runlify-ext-docker
```

Добавляет следующие команды:
- `runlify docker build` - Сборка Docker-образа
- `runlify docker push` - Отправка образа в реестр
- `runlify docker run` - Запуск контейнера

### runlify-ext-aws

Расширение для интеграции с AWS.

```bash
runlify ext install runlify-ext-aws
```

Добавляет следующие команды:
- `runlify aws deploy` - Развертывание в AWS
- `runlify aws s3 sync` - Синхронизация с S3
- `runlify aws lambda update` - Обновление функции Lambda

### runlify-ext-database

Расширение для работы с базами данных.

```bash
runlify ext install runlify-ext-database
```

Добавляет следующие команды:
- `runlify db migrate` - Выполнение миграций
- `runlify db seed` - Заполнение базы данных тестовыми данными
- `runlify db backup` - Создание резервной копии базы данных

## Создание собственных расширений

Вы можете создавать собственные расширения для Runlify CLI.

### Структура расширения

Расширение должно иметь следующую структуру:

```
my-extension/
├── src/
│   ├── commands/
│   │   └── myCommand.ts
│   ├── generators/
│   │   └── myGenerator.ts
│   ├── hooks/
│   │   └── myHook.ts
│   └── index.ts
├── package.json
└── README.md
```

### Файл package.json

Файл `package.json` должен содержать следующие поля:

```json
{
  "name": "runlify-ext-myextension",
  "version": "1.0.0",
  "description": "Мое расширение для Runlify CLI",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "keywords": [
    "runlify",
    "runlify-extension"
  ],
  "runlify": {
    "type": "extension",
    "commands": [
      "myCommand"
    ],
    "hooks": [
      "afterInit",
      "beforeDeploy"
    ]
  },
  "dependencies": {
    "runlify": "^0.0.709"
  }
}
```

### Создание команды

Пример создания новой команды:

```typescript
// src/commands/myCommand.ts
import { GluegunCommand } from 'gluegun';

const command: GluegunCommand = {
  name: 'myCommand',
  description: 'Моя пользовательская команда',
  run: async (toolbox) => {
    const { print } = toolbox;
    print.info('Выполнение моей команды');
    // Ваш код здесь
  },
};

export default command;
```

### Регистрация команды

Регистрация команды в основном файле расширения:

```typescript
// src/index.ts
import { GluegunToolbox } from 'gluegun';
import myCommand from './commands/myCommand';

export default (toolbox: GluegunToolbox) => {
  toolbox.myCommand = () => {
    toolbox.print.info('Моя команда инициализирована');
  };
  
  // Регистрация команды
  toolbox.commands.register(myCommand);
};
```

### Публикация расширения

Для публикации расширения в npm:

```bash
npm login
npm publish
```

## Рекомендации по работе с расширениями

- Используйте официальные расширения для стандартных задач
- Создавайте собственные расширения для специфичных для вашей организации задач
- Следите за обновлениями расширений для получения новых функций и исправлений ошибок
- Документируйте конфигурацию и использование расширений в вашем проекте
- При разработке расширений следуйте принципам модульности и переиспользуемости 