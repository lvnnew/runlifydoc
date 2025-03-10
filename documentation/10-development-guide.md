# Руководство по разработке

## Обзор

Этот раздел предназначен для разработчиков, которые хотят внести свой вклад в развитие Runlify CLI или разрабатывать собственные расширения и интеграции. Здесь вы найдете информацию о структуре проекта, процессе разработки, тестировании и публикации.

## Структура проекта

Проект Runlify CLI имеет следующую структуру:

```
runlify/
├── bin/                  # Исполняемые файлы
├── build/                # Скомпилированный код (генерируется)
├── docs/                 # Документация
├── src/                  # Исходный код
│   ├── commands/         # Команды CLI
│   ├── extensions/       # Расширения
│   ├── projectsGeneration/ # Генерация проектов
│   ├── templates/        # Шаблоны проектов
│   ├── utils/            # Утилиты
│   ├── cli.ts            # Точка входа CLI
│   └── index.ts          # Точка входа API
├── tests/                # Тесты
├── .eslintrc.js          # Конфигурация ESLint
├── .gitignore            # Игнорируемые файлы Git
├── package.json          # Манифест пакета
├── tsconfig.json         # Конфигурация TypeScript
└── README.md             # Readme проекта
```

## Настройка окружения разработки

### Предварительные требования

- Node.js (версия 14 или выше)
- Yarn (предпочтительно) или npm
- Git

### Клонирование репозитория

```bash
git clone https://github.com/yourusername/runlify.git
cd runlify
```

### Установка зависимостей

```bash
yarn install
# или
npm install
```

### Сборка проекта

```bash
yarn build
# или
npm run build
```

### Запуск в режиме разработки

```bash
# Запуск CLI
node bin/runlify <command>

# Запуск с отладкой
node --inspect-brk bin/runlify <command>
```

## Разработка новых функций

### Создание новой команды

1. Создайте новый файл в директории `src/commands/`:

```typescript
// src/commands/myCommand.ts
import { GluegunCommand } from 'gluegun';

const command: GluegunCommand = {
  name: 'myCommand',
  description: 'Моя новая команда',
  run: async (toolbox) => {
    const { print, parameters } = toolbox;
    const { options, args } = parameters;
    
    print.info('Выполнение моей команды');
    print.info(`Аргументы: ${args.join(', ')}`);
    print.info(`Опции: ${JSON.stringify(options)}`);
    
    // Ваш код здесь
  },
};

export default command;
```

2. Зарегистрируйте команду в `src/cli.ts`:

```typescript
// src/cli.ts
import { build } from 'gluegun';

// ... существующий код ...

// Регистрация команд
const cli = build()
  .brand('runlify')
  .src(__dirname)
  .plugins('./node_modules', { matching: 'runlify-*', hidden: true })
  .help()
  .version()
  .create();

// ... существующий код ...
```

### Создание нового генератора

1. Создайте новый файл в директории `src/projectsGeneration/`:

```typescript
// src/projectsGeneration/myGenerator.ts
import { GluegunToolbox } from 'gluegun';

export default {
  name: 'myGenerator',
  description: 'Генерирует мой компонент',
  run: async (toolbox: GluegunToolbox, options: any) => {
    const { template, print, filesystem } = toolbox;
    const { name, path = 'src/components' } = options;
    
    if (!name) {
      print.error('Необходимо указать имя компонента');
      return;
    }
    
    // Генерация файлов
    await template.generate({
      template: 'myComponent.ts.ejs',
      target: `${path}/${name}.ts`,
      props: { name },
    });
    
    print.success(`Компонент ${name} успешно создан в ${path}/${name}.ts`);
  },
};
```

2. Создайте шаблон в директории `src/templates/`:

```typescript
// src/templates/myComponent.ts.ejs
export interface <%= props.name %>Props {
  // Свойства компонента
}

export class <%= props.name %> {
  constructor(private props: <%= props.name %>Props) {}
  
  // Методы компонента
}
```

### Создание нового расширения

1. Создайте новую директорию для расширения:

```bash
mkdir -p src/extensions/myExtension
```

2. Создайте основной файл расширения:

```typescript
// src/extensions/myExtension/index.ts
import { GluegunToolbox } from 'gluegun';

export default (toolbox: GluegunToolbox) => {
  const { print } = toolbox;
  
  // Добавление новых функций в toolbox
  toolbox.myExtension = {
    sayHello: (name: string) => {
      print.info(`Привет, ${name}!`);
    },
  };
  
  // Регистрация команд
  toolbox.commands.register({
    name: 'myExtension',
    description: 'Моё расширение',
    run: async (toolbox) => {
      print.info('Выполнение команды расширения');
    },
  });
};
```

## Тестирование

### Запуск тестов

```bash
yarn test
# или
npm test
```

### Запуск тестов с покрытием

```bash
yarn coverage
# или
npm run coverage
```

### Написание тестов

Тесты размещаются в директории `tests/` и используют Jest:

```typescript
// tests/myCommand.test.ts
import { system } from 'gluegun';

const src = `${process.cwd()}/bin/runlify`;

describe('Команда myCommand', () => {
  test('выполняется успешно', async () => {
    const output = await system.run(`${src} myCommand`);
    expect(output).toContain('Выполнение моей команды');
  });
  
  test('обрабатывает аргументы', async () => {
    const output = await system.run(`${src} myCommand arg1 arg2`);
    expect(output).toContain('Аргументы: arg1, arg2');
  });
  
  test('обрабатывает опции', async () => {
    const output = await system.run(`${src} myCommand --option1 value1`);
    expect(output).toContain('"option1":"value1"');
  });
});
```

## Отладка

### Использование отладчика

Для отладки используйте флаг `--inspect-brk`:

```bash
node --inspect-brk bin/runlify <command>
```

Затем подключитесь к отладчику с помощью Chrome DevTools или VS Code.

### Логирование

Для отладки используйте модуль `logger`:

```typescript
import { logger } from '../utils/logger';

// Различные уровни логирования
logger.debug('Отладочная информация');
logger.info('Информационное сообщение');
logger.warn('Предупреждение');
logger.error('Ошибка');

// Установка уровня логирования
logger.setLevel('debug');
```

## Публикация

### Подготовка к публикации

1. Обновите версию в `package.json`:

```bash
yarn version --new-version x.y.z
# или
npm version x.y.z
```

2. Обновите CHANGELOG.md с описанием изменений.

3. Соберите проект:

```bash
yarn build
# или
npm run build
```

### Публикация в npm

```bash
npm login
npm publish
```

### Создание релиза на GitHub

1. Создайте тег:

```bash
git tag -a vx.y.z -m "Версия x.y.z"
git push origin vx.y.z
```

2. Создайте релиз на GitHub с описанием изменений.

## Рекомендации по разработке

### Стиль кода

- Следуйте стилю кода, определенному в `.eslintrc.js` и `.prettierrc`
- Используйте TypeScript для типизации
- Пишите документацию к публичным API
- Используйте асинхронные функции вместо колбэков

### Управление зависимостями

- Минимизируйте количество зависимостей
- Регулярно обновляйте зависимости
- Используйте фиксированные версии для критических зависимостей

### Обработка ошибок

- Используйте try/catch для обработки ошибок
- Логируйте ошибки с контекстом
- Предоставляйте понятные сообщения об ошибках пользователю

### Документация

- Документируйте новые функции и изменения
- Обновляйте README.md и документацию при добавлении новых функций
- Используйте JSDoc для документирования функций и классов

## Ресурсы для разработчиков

- [Документация Gluegun](https://github.com/infinitered/gluegun/tree/master/docs)
- [TypeScript Documentation](https://www.typescriptlang.org/docs/)
- [Jest Documentation](https://jestjs.io/docs/en/getting-started)
- [Node.js Documentation](https://nodejs.org/en/docs/) 