# Часто задаваемые вопросы

## Общие вопросы

### Что такое Runlify CLI?

Runlify CLI - это инструмент командной строки, предназначенный для упрощения процесса разработки, развертывания и управления проектами. Он предоставляет разработчикам удобный интерфейс для выполнения различных задач, связанных с жизненным циклом проекта.

### Для каких проектов подходит Runlify CLI?

Runlify CLI подходит для различных типов проектов, включая веб-приложения, мобильные приложения, серверные приложения и микросервисы. Он особенно полезен для проектов, использующих JavaScript/TypeScript, React, Node.js и другие современные технологии.

### Какие операционные системы поддерживаются?

Runlify CLI поддерживает следующие операционные системы:
- Windows
- macOS
- Linux

### Какая версия Node.js требуется?

Для работы Runlify CLI требуется Node.js версии 14.0.0 или выше.

## Установка и обновление

### Как установить Runlify CLI?

Вы можете установить Runlify CLI с помощью npm или yarn:

```bash
# Установка с помощью npm
npm install -g runlify

# Установка с помощью yarn
yarn global add runlify
```

### Как обновить Runlify CLI до последней версии?

Вы можете обновить Runlify CLI с помощью следующих команд:

```bash
# Обновление с помощью npm
npm update -g runlify

# Обновление с помощью yarn
yarn global upgrade runlify

# Обновление с помощью самого Runlify CLI
runlify update
```

### Как проверить текущую версию Runlify CLI?

Для проверки текущей версии используйте команду:

```bash
runlify version
# или
runlify --version
```

### Возникают проблемы с установкой. Что делать?

Если у вас возникают проблемы с установкой, попробуйте следующие решения:

1. Убедитесь, что у вас установлена поддерживаемая версия Node.js:
   ```bash
   node --version
   ```

2. Попробуйте установить с правами администратора:
   ```bash
   sudo npm install -g runlify
   ```

3. Очистите кэш npm:
   ```bash
   npm cache clean --force
   ```

4. Если проблемы сохраняются, проверьте журнал ошибок и обратитесь в службу поддержки.

## Использование

### Как создать новый проект с помощью Runlify CLI?

Для создания нового проекта используйте команду `init`:

```bash
runlify init my-project --template react
```

### Какие шаблоны проектов доступны?

Runlify CLI предоставляет следующие шаблоны проектов:
- `basic` - Базовый шаблон с минимальной конфигурацией
- `react` - Шаблон для React-приложения с TypeScript
- `node-api` - Шаблон для Node.js API с Express и TypeScript
- `fullstack` - Шаблон для fullstack-приложения с React и Node.js

### Как генерировать компоненты и модули?

Для генерации компонентов и модулей используйте команду `generate` (или сокращенно `g`):

```bash
# Генерация компонента
runlify generate component Button

# Генерация модуля
runlify g module Auth
```

### Как переключаться между окружениями?

Для переключения между окружениями используйте команду `env switch`:

```bash
runlify env switch production
```

### Как настроить конфигурацию проекта?

Конфигурация проекта хранится в файле `runlify.json` в корневой директории проекта. Вы можете редактировать этот файл вручную или использовать команду `config`:

```bash
# Установка значения конфигурации
runlify config set apiUrl https://api.example.com

# Получение значения конфигурации
runlify config get apiUrl
```

## Расширения

### Как установить расширение?

Для установки расширения используйте команду `extension install` (или сокращенно `ext install`):

```bash
runlify ext install runlify-ext-docker
```

### Как создать собственное расширение?

Для создания собственного расширения следуйте инструкциям в разделе [Расширения](./08-extensions.md#создание-собственных-расширений).

### Как обновить установленные расширения?

Для обновления установленных расширений используйте команду:

```bash
# Обновление всех расширений
runlify ext update

# Обновление конкретного расширения
runlify ext update runlify-ext-docker
```

## Развертывание

### Как развернуть проект?

Для развертывания проекта используйте команду `deploy`:

```bash
runlify deploy --env production
```

### Какие платформы поддерживаются для развертывания?

Runlify CLI поддерживает развертывание на различные платформы, включая:
- AWS
- Azure
- Google Cloud Platform
- Heroku
- Netlify
- Vercel

Для некоторых платформ может потребоваться установка соответствующих расширений.

### Как настроить автоматическое развертывание?

Для настройки автоматического развертывания вы можете использовать CI/CD-пайплайны (например, GitHub Actions, GitLab CI, Jenkins) с Runlify CLI:

```yaml
# Пример для GitHub Actions
name: Deploy

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: '14'
      - run: npm install -g runlify
      - run: runlify deploy --env production
```

## Устранение неполадок

### Команда не найдена. Что делать?

Если вы получаете ошибку "команда не найдена", убедитесь, что:

1. Runlify CLI установлен глобально:
   ```bash
   npm list -g runlify
   ```

2. Директория глобальных пакетов npm находится в переменной PATH:
   ```bash
   echo $PATH
   ```

3. Попробуйте переустановить Runlify CLI:
   ```bash
   npm uninstall -g runlify
   npm install -g runlify
   ```

### Как включить отладочный режим?

Для включения отладочного режима используйте флаг `--verbose`:

```bash
runlify <command> --verbose
```

Также вы можете установить переменную окружения `RUNLIFY_LOG_LEVEL`:

```bash
export RUNLIFY_LOG_LEVEL=debug
runlify <command>
```

### Где находятся логи Runlify CLI?

Логи Runlify CLI хранятся в следующих директориях:

- Windows: `%APPDATA%\runlify\logs`
- macOS: `~/Library/Application Support/runlify/logs`
- Linux: `~/.config/runlify/logs`

### Как сбросить конфигурацию Runlify CLI?

Для сброса конфигурации Runlify CLI удалите файл конфигурации:

- Windows: `%APPDATA%\runlify\config.json`
- macOS: `~/Library/Application Support/runlify/config.json`
- Linux: `~/.config/runlify/config.json`

## Разработка и вклад в проект

### Как внести вклад в развитие Runlify CLI?

Если вы хотите внести свой вклад в развитие Runlify CLI, следуйте инструкциям в разделе [Руководство по разработке](./10-development-guide.md).

### Где можно сообщить о проблеме или предложить новую функцию?

Вы можете сообщить о проблеме или предложить новую функцию на GitHub-репозитории проекта:

[https://github.com/yourusername/runlify/issues](https://github.com/yourusername/runlify/issues)

### Как связаться с командой разработчиков?

Вы можете связаться с командой разработчиков следующими способами:

- GitHub Issues: [https://github.com/yourusername/runlify/issues](https://github.com/yourusername/runlify/issues)
- Email: support@runlify.com
- Twitter: [@runlify](https://twitter.com/runlify)
- Discord: [Runlify Community](https://discord.gg/runlify)

## Лицензия и коммерческое использование

### Под какой лицензией распространяется Runlify CLI?

Runlify CLI распространяется под лицензией MIT. Подробности смотрите в файле [LICENSE](../LICENSE) в корне проекта.

### Можно ли использовать Runlify CLI в коммерческих проектах?

Да, Runlify CLI можно использовать в коммерческих проектах. Лицензия MIT позволяет использовать, копировать, изменять и распространять программное обеспечение без ограничений, включая коммерческое использование. 