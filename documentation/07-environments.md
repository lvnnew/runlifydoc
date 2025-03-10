# Управление окружениями

## Обзор

Runlify CLI предоставляет мощные инструменты для управления различными окружениями разработки, тестирования и производства. Это позволяет легко переключаться между окружениями, настраивать специфичные для окружения параметры и обеспечивать согласованность конфигурации между различными средами. В этом разделе описаны различные аспекты управления окружениями с помощью Runlify CLI.

## Основные окружения

По умолчанию Runlify CLI поддерживает следующие окружения:

- **development** - Окружение для локальной разработки
- **testing** - Окружение для тестирования
- **staging** - Предпроизводственное окружение
- **production** - Производственное окружение

## Команды для управления окружениями

Для управления окружениями используйте команду `runlify env`:

```bash
# Список всех доступных окружений
runlify env list

# Создание нового окружения
runlify env create <name>

# Переключение на другое окружение
runlify env switch <name>

# Удаление окружения
runlify env delete <name>

# Отображение текущего активного окружения
runlify env current

# Клонирование окружения
runlify env clone <source> <target>
```

## Конфигурация окружений

Конфигурация для различных окружений хранится в файле `runlify.json` в корневой директории проекта:

```json
{
  "environments": {
    "development": {
      "apiUrl": "http://localhost:3000/api",
      "debug": true,
      "logLevel": "debug"
    },
    "testing": {
      "apiUrl": "https://test-api.example.com",
      "debug": true,
      "logLevel": "info"
    },
    "staging": {
      "apiUrl": "https://staging-api.example.com",
      "debug": false,
      "logLevel": "warn"
    },
    "production": {
      "apiUrl": "https://api.example.com",
      "debug": false,
      "logLevel": "error"
    }
  }
}
```

## Переменные окружения

Для каждого окружения вы можете определить специфичные переменные окружения в файле `.env.<environment>`:

```
# .env.development
API_URL=http://localhost:3000/api
DEBUG=true
LOG_LEVEL=debug

# .env.production
API_URL=https://api.example.com
DEBUG=false
LOG_LEVEL=error
```

Runlify CLI автоматически загружает соответствующий файл `.env` при переключении окружений.

## Доступ к конфигурации окружения в коде

В вашем коде вы можете получить доступ к конфигурации текущего окружения с помощью API Runlify:

```typescript
import { environment } from 'runlify';

// Получение текущего окружения
const currentEnv = environment.current();
console.log(`Текущее окружение: ${currentEnv}`);

// Получение значения конфигурации для текущего окружения
const apiUrl = environment.get('apiUrl');
console.log(`API URL: ${apiUrl}`);

// Проверка, является ли текущее окружение производственным
if (environment.isProduction()) {
  console.log('Запущено в производственном окружении');
}
```

## Переключение окружений при запуске команд

Вы можете временно переключить окружение при выполнении команды с помощью флага `--env`:

```bash
runlify deploy --env production
```

## Создание пользовательских окружений

Помимо стандартных окружений, вы можете создавать собственные окружения для специфических нужд:

```bash
runlify env create demo
```

После создания окружения вы можете настроить его в файле `runlify.json`:

```json
{
  "environments": {
    "demo": {
      "apiUrl": "https://demo-api.example.com",
      "debug": true,
      "logLevel": "info",
      "features": {
        "newFeature": true
      }
    }
  }
}
```

## Наследование конфигурации окружений

Окружения могут наследовать конфигурацию от других окружений:

```json
{
  "environments": {
    "base": {
      "timeout": 30000,
      "retries": 3
    },
    "development": {
      "extends": "base",
      "apiUrl": "http://localhost:3000/api",
      "debug": true
    },
    "production": {
      "extends": "base",
      "apiUrl": "https://api.example.com",
      "debug": false
    }
  }
}
```

## Развертывание в различных окружениях

Для развертывания проекта в определенном окружении используйте команду `runlify deploy` с указанием окружения:

```bash
runlify deploy --env production
```

## Переменные окружения для CI/CD

Для использования в CI/CD-пайплайнах вы можете определить переменные окружения с префиксом `RUNLIFY_ENV_`:

```
RUNLIFY_ENV_API_URL=https://ci-api.example.com
RUNLIFY_ENV_DEBUG=false
```

Эти переменные будут иметь приоритет над значениями, определенными в файлах конфигурации.

## Рекомендации по управлению окружениями

- Используйте разные окружения для разных этапов разработки и тестирования
- Храните чувствительные данные (токены, пароли) в переменных окружения, а не в файлах конфигурации
- Используйте наследование конфигурации для уменьшения дублирования
- Регулярно проверяйте конфигурацию всех окружений на согласованность
- Документируйте специфичные для окружения настройки и требования 