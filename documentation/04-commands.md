# Команды CLI

## Обзор

Runlify CLI предоставляет набор команд для управления проектами, генерации кода и взаимодействия с различными сервисами. В этом разделе описаны все доступные команды, их параметры и примеры использования.

## Основные команды

### `runlify help`

Отображает справку по доступным командам.

```bash
runlify help
runlify help <command>
```

### `runlify version`

Отображает текущую версию Runlify CLI.

```bash
runlify version
```

### `runlify init`

Инициализирует новый проект Runlify в текущей директории.

```bash
runlify init [name]
```

Параметры:
- `name` - Имя проекта (опционально)

Флаги:
- `--template <template>` - Шаблон проекта (по умолчанию: basic)
- `--force` - Перезаписать существующие файлы

### `runlify generate` (или `g`)

Генерирует компоненты, модули или другие элементы проекта.

```bash
runlify generate <type> <name> [options]
runlify g <type> <name> [options]
```

Типы генераторов:
- `component` - Генерирует компонент
- `module` - Генерирует модуль
- `service` - Генерирует сервис
- `model` - Генерирует модель данных

Примеры:
```bash
runlify generate component Button
runlify g module Auth
```

### `runlify deploy`

Развертывает проект в указанном окружении.

```bash
runlify deploy [environment]
```

Параметры:
- `environment` - Окружение для развертывания (по умолчанию: production)

Флаги:
- `--force` - Принудительное развертывание
- `--skip-build` - Пропустить этап сборки

### `runlify login`

Аутентификация в сервисе Runlify.

```bash
runlify login
```

### `runlify logout`

Выход из сервиса Runlify.

```bash
runlify logout
```

### `runlify config`

Управление конфигурацией Runlify CLI.

```bash
runlify config get <key>
runlify config set <key> <value>
runlify config list
```

## Дополнительные команды

### `runlify env`

Управление окружениями проекта.

```bash
runlify env list
runlify env create <name>
runlify env switch <name>
runlify env delete <name>
```

### `runlify update`

Обновление Runlify CLI до последней версии.

```bash
runlify update
```

## Глобальные флаги

Следующие флаги могут быть использованы с любой командой:

- `--verbose` - Подробный вывод
- `--quiet` - Минимальный вывод
- `--json` - Вывод в формате JSON
- `--help` - Отображение справки по команде

## Примеры использования

### Создание нового проекта

```bash
runlify init my-awesome-project --template react
```

### Генерация компонентов

```bash
runlify generate component Header --style css
runlify g component Footer --style scss
```

### Управление окружениями

```bash
runlify env create staging
runlify env switch staging
runlify deploy
```

## Расширение команд

Runlify CLI поддерживает расширения, которые могут добавлять новые команды. Подробнее о расширениях можно узнать в разделе [Расширения](./08-extensions.md). 