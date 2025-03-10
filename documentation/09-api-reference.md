# API Reference

## Обзор

Runlify CLI предоставляет программный API, который можно использовать в ваших скриптах и приложениях для взаимодействия с функциональностью Runlify. В этом разделе описаны доступные API-модули, их методы и примеры использования.

## Установка

Для использования API Runlify в вашем проекте установите пакет `runlify`:

```bash
npm install runlify
# или
yarn add runlify
```

## Основные модули API

### Модуль `runlify`

Основной модуль, предоставляющий доступ ко всем функциям Runlify.

```typescript
import { runlify } from 'runlify';

// Инициализация проекта
await runlify.init({
  name: 'my-project',
  template: 'react',
  force: false,
  skipInstall: false,
});

// Генерация компонента
await runlify.generate({
  type: 'component',
  name: 'Button',
  options: {
    style: 'css',
  },
});
```

### Модуль `environment`

Модуль для работы с окружениями.

```typescript
import { environment } from 'runlify';

// Получение текущего окружения
const currentEnv = environment.current();

// Получение значения конфигурации для текущего окружения
const apiUrl = environment.get('apiUrl');

// Проверка, является ли текущее окружение производственным
if (environment.isProduction()) {
  console.log('Запущено в производственном окружении');
}

// Переключение окружения
await environment.switch('staging');

// Создание нового окружения
await environment.create('demo', {
  apiUrl: 'https://demo-api.example.com',
  debug: true,
});
```

### Модуль `config`

Модуль для работы с конфигурацией.

```typescript
import { config } from 'runlify';

// Получение значения конфигурации
const apiUrl = config.get('apiUrl');

// Установка значения конфигурации
config.set('apiUrl', 'https://api.example.com');

// Получение всей конфигурации
const allConfig = config.getAll();

// Сохранение конфигурации
await config.save();
```

### Модуль `project`

Модуль для работы с проектами.

```typescript
import { project } from 'runlify';

// Получение информации о проекте
const projectInfo = project.info();

// Проверка, является ли директория проектом Runlify
const isRunlifyProject = project.isRunlifyProject();

// Получение корневой директории проекта
const rootDir = project.getRootDir();

// Получение зависимостей проекта
const dependencies = project.getDependencies();
```

### Модуль `template`

Модуль для работы с шаблонами.

```typescript
import { template } from 'runlify';

// Получение списка доступных шаблонов
const templates = await template.list();

// Получение информации о шаблоне
const templateInfo = await template.info('react');

// Добавление нового шаблона
await template.add('./path/to/my-template');

// Удаление шаблона
await template.remove('my-template');
```

### Модуль `extension`

Модуль для работы с расширениями.

```typescript
import { extension } from 'runlify';

// Получение списка установленных расширений
const extensions = await extension.list();

// Установка расширения
await extension.install('runlify-ext-docker');

// Удаление расширения
await extension.remove('runlify-ext-docker');

// Получение информации о расширении
const extensionInfo = await extension.info('runlify-ext-docker');
```

### Модуль `logger`

Модуль для логирования.

```typescript
import { logger } from 'runlify';

// Логирование информации
logger.info('Информационное сообщение');

// Логирование предупреждения
logger.warn('Предупреждение');

// Логирование ошибки
logger.error('Ошибка');

// Логирование отладочной информации
logger.debug('Отладочная информация');

// Установка уровня логирования
logger.setLevel('debug');
```

### Модуль `utils`

Модуль с утилитами.

```typescript
import { utils } from 'runlify';

// Форматирование строки
const formattedString = utils.format.camelCase('hello-world'); // helloWorld

// Проверка версии Node.js
const isCompatible = utils.node.checkVersion('>=14.0.0');

// Выполнение команды
const result = await utils.shell.exec('npm install');

// Чтение файла
const fileContent = await utils.fs.readFile('./package.json');

// Запись файла
await utils.fs.writeFile('./config.json', JSON.stringify({ key: 'value' }));
```

## Примеры использования API

### Создание скрипта для инициализации проекта

```typescript
// init-project.ts
import { runlify, environment, config } from 'runlify';

async function initProject() {
  // Инициализация проекта
  await runlify.init({
    name: 'my-awesome-project',
    template: 'react',
    force: false,
    skipInstall: false,
  });

  // Настройка окружений
  await environment.create('staging', {
    apiUrl: 'https://staging-api.example.com',
    debug: true,
  });

  await environment.create('production', {
    apiUrl: 'https://api.example.com',
    debug: false,
  });

  // Установка глобальной конфигурации
  config.set('defaultTemplate', 'react');
  await config.save();

  // Генерация компонентов
  await runlify.generate({
    type: 'component',
    name: 'Header',
    options: {
      style: 'scss',
    },
  });

  await runlify.generate({
    type: 'component',
    name: 'Footer',
    options: {
      style: 'scss',
    },
  });

  console.log('Проект успешно инициализирован!');
}

initProject().catch(console.error);
```

### Создание скрипта для развертывания

```typescript
// deploy.ts
import { runlify, environment, logger } from 'runlify';

async function deploy(env = 'production') {
  logger.info(`Развертывание в окружении ${env}`);

  // Переключение на нужное окружение
  await environment.switch(env);

  // Сборка проекта
  logger.info('Сборка проекта...');
  await runlify.build();

  // Развертывание
  logger.info('Развертывание...');
  await runlify.deploy({
    environment: env,
    force: false,
    skipBuild: true, // Мы уже выполнили сборку
  });

  logger.info('Развертывание успешно завершено!');
}

// Получение окружения из аргументов командной строки
const env = process.argv[2] || 'production';
deploy(env).catch((error) => {
  logger.error('Ошибка при развертывании:', error);
  process.exit(1);
});
```

## Обработка ошибок

При использовании API Runlify рекомендуется обрабатывать возможные ошибки:

```typescript
import { runlify, logger } from 'runlify';

async function generateComponent(name) {
  try {
    await runlify.generate({
      type: 'component',
      name,
      options: {
        style: 'css',
      },
    });
    logger.info(`Компонент ${name} успешно создан`);
  } catch (error) {
    logger.error(`Ошибка при создании компонента ${name}:`, error);
    throw error;
  }
}

generateComponent('Button').catch(() => process.exit(1));
```

## Типы данных

Runlify API предоставляет TypeScript-типы для всех своих функций и объектов:

```typescript
import { 
  RunlifyOptions, 
  EnvironmentConfig, 
  GenerateOptions, 
  DeployOptions,
  ProjectInfo,
  TemplateInfo,
  ExtensionInfo
} from 'runlify';

// Использование типов
const options: RunlifyOptions = {
  name: 'my-project',
  template: 'react',
  force: false,
  skipInstall: false,
};

const envConfig: EnvironmentConfig = {
  apiUrl: 'https://api.example.com',
  debug: false,
  logLevel: 'info',
};
```

## Рекомендации по использованию API

- Используйте TypeScript для получения преимуществ типизации и автодополнения
- Обрабатывайте ошибки с помощью try/catch или цепочек Promise
- Используйте модуль `logger` для логирования вместо `console.log`
- Создавайте переиспользуемые функции для часто выполняемых операций
- Документируйте ваши скрипты, использующие API Runlify 