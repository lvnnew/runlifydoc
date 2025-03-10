# Примеры использования

В этом разделе представлены практические примеры использования Runlify для различных сценариев разработки.

## Базовая структура проекта

```typescript
// src/meta/addCatalogs.ts
import { SystemMetaBuilder } from 'runlify';

const addCatalogs = (system: SystemMetaBuilder) => {
  // Определение сущностей
  addTeams(system);
  addClubs(system);
  addPlayers(system);
  addMatches(system);
};

export default addCatalogs;
```

## Примеры сущностей

### Команды (Teams)

```typescript
const addTeams = (system: SystemMetaBuilder) => {
  const teams = system.addCatalog('teams')
    .setTitle('Команды')
    .addScalarField('name', 'string')
    .setTitleField('name')
    .setRequired('name')
    .addScalarField('dateOfBirthFrom', 'number')
    .setRequired('dateOfBirthFrom')
    .addScalarField('dateOfBirthTo', 'number')
    .addLinkField('managers', 'managerId', 'Менеджер')
    .setRequired('managerId')
    .addLinkField('clubs', 'clubId', 'Клуб')
    .setRequired('clubId')
    .addLinkField('files', 'logoId', 'Логотип команды')
    .enableSearch()
    .enableAudit()
    .setCreatableByUser(true)
    .setUpdatableByUser(true)
    .setDeletable(false)
    .addPermission('canApprove', 'approve')
    .setSort('name', 'ASC');

  return teams;
};
```

### Клубы (Clubs)

```typescript
const addClubs = (system: SystemMetaBuilder) => {
  const clubs = system.addCatalog('clubs')
    .setTitle('Клубы')
    .addScalarField('name', 'string')
    .setTitleField('name')
    .setRequired('name')
    .addLinkField('managers', 'managerId', 'Менеджер')
    .setRequired('managerId')
    .enableSearch()
    .enableAudit()
    .setCreatableByUser(true)
    .setUpdatableByUser(true)
    .setDeletable(false);

  return clubs;
};
```

### Игроки (Players)

```typescript
const addPlayers = (system: SystemMetaBuilder) => {
  const players = system.addCatalog('players')
    .setTitle('Игроки')
    .addScalarField('name', 'string')
    .setTitleField('name')
    .setRequired('name')
    .addScalarField('birthDate', 'date')
    .setRequired('birthDate')
    .addScalarField('isActive', 'boolean')
    .setDefault('isActive', true)
    .addLinkField('teams', 'teamId', 'Команда')
    .setRequired('teamId')
    .addLinkField('clubs', 'clubId', 'Клуб')
    .setRequired('clubId')
    .enableSearch()
    .enableAudit()
    .setCreatableByUser(true)
    .setUpdatableByUser(true)
    .setDeletable(false)
    .setSort('name', 'ASC');

  return players;
};
```

## Примеры конфигурации

### Настройка меню

```typescript
// src/meta/addMenu.ts
import { SystemMetaBuilder } from 'runlify';

const addMenu = (system: SystemMetaBuilder) => {
  system.addMenu('main')
    .addItem('teams', 'Команды', 'Group')
    .addItem('clubs', 'Клубы', 'Business')
    .addItem('players', 'Игроки', 'Person')
    .addItem('matches', 'Матчи', 'SportsSoccer');
};

export default addMenu;
```

### Настройка прав доступа

```typescript
const configurePermissions = (entity: CatalogBuilder) => {
  // Базовые права
  entity.setCreatableByUser(true);
  entity.setUpdatableByUser(true);
  entity.setDeletable(false);
  entity.enableSearch();
  entity.enableAudit();

  // Кастомные права
  entity.addPermission('canApprove', 'approve');
  entity.addPermission('canReject', 'reject');
};
```

## Примеры использования API

### REST API

```typescript
// Получение списка команд
const teams = await fetch('/api/teams?filter[name]=test&sort=name&page=1&per_page=10');

// Создание команды
const newTeam = await fetch('/api/teams', {
  method: 'POST',
  body: JSON.stringify({
    name: 'Новая команда',
    managerId: '123',
    clubId: '456'
  })
});

// Обновление команды
const updatedTeam = await fetch('/api/teams/123', {
  method: 'PUT',
  body: JSON.stringify({
    name: 'Обновленная команда'
  })
});
```

### GraphQL API

```typescript
// Запрос списка команд
const query = `
  query GetTeams {
    teams {
      id
      name
      manager {
        id
        name
      }
      club {
        id
        name
      }
    }
  }
`;

// Мутация создания команды
const mutation = `
  mutation CreateTeam($input: CreateTeamInput!) {
    createTeam(input: $input) {
      id
      name
      manager {
        id
        name
      }
      club {
        id
        name
      }
    }
  }
`;
```

## Примеры интеграции с React Admin

### Компонент списка команд

```typescript
// src/components/TeamsList.tsx
import { List, Datagrid, TextField, ReferenceField } from 'react-admin';

export const TeamsList = () => (
  <List>
    <Datagrid>
      <TextField source="name" />
      <ReferenceField source="managerId" reference="managers">
        <TextField source="name" />
      </ReferenceField>
      <ReferenceField source="clubId" reference="clubs">
        <TextField source="name" />
      </ReferenceField>
    </Datagrid>
  </List>
);
```

### Форма создания команды

```typescript
// src/components/TeamForm.tsx
import { Create, SimpleForm, TextInput, ReferenceInput } from 'react-admin';

export const TeamCreate = () => (
  <Create>
    <SimpleForm>
      <TextInput source="name" />
      <ReferenceInput source="managerId" reference="managers">
        <TextInput source="name" />
      </ReferenceInput>
      <ReferenceInput source="clubId" reference="clubs">
        <TextInput source="name" />
      </ReferenceInput>
    </SimpleForm>
  </Create>
);
```

## Следующие шаги

- Ознакомьтесь с лучшими практиками в разделе [Лучшие практики](08-best-practices.md)
- При возникновении проблем обратитесь к разделу [Устранение неполадок](09-troubleshooting.md) 