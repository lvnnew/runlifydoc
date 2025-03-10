# Примеры использования

В этом разделе представлены практические примеры использования Runlify для различных сценариев разработки. Примеры основаны на реальном проекте спортивной системы.

## Базовая структура проекта

```typescript
// src/meta/addCatalogs.ts
import { SystemMetaBuilder, getFilesCatalog } from 'runlify';

const addCatalogs = (system: SystemMetaBuilder) => {
  const files = getFilesCatalog(system);
  
  // Определение сущностей
  addTeams(system, files);
  addClubs(system);
  addPlayers(system, files);
  addMatches(system);
  addReports(system);
};

export default addCatalogs;
```

## Примеры сущностей

### Команды (Teams)

```typescript
const addTeams = (system: SystemMetaBuilder, files: CatalogBuilder) => {
  const teams = system.addCatalog('teams');
  teams.setNeedFor('Таблица команд');
  teams.setTitles({
    en: { plural: 'Teams', singular: 'Team' },
    ru: { plural: 'Команды', singular: 'Команда' }
  });

  // Основные поля
  teams.addField('title', 'Название', { isTitleField: true })
    .setType('string')
    .setRequired();

  teams.addField('dateOfBirthFrom', 'Год рождения с')
    .setType('int')
    .setRequired();

  teams.addField('dateOfBirthTo', 'По год рождения')
    .setType('int');

  // Связи
  teams.addLinkField('managers', 'createdByManagerId', 'Создано менеджером')
    .setRequired()
    .setNotUpdatableByUser(undefined, 'ctx.service(\'profile\').getManagerId()');

  teams.addLinkField('managers', 'lastChangedByManagerId', 'Изменено менеджером')
    .setNotUpdatableByUser(undefined);

  teams.addLinkField('clubs', 'clubId', 'Клуб')
    .setRequired();

  teams.addLinkField(files, 'fileId', 'Логотип команды');

  return teams;
};
```

### Клубы (Clubs)

```typescript
const addClubs = (system: SystemMetaBuilder) => {
  const clubs = system.addCatalog('clubs');
  clubs.setNeedFor('Клубы');
  clubs.setTitles({
    en: { plural: 'Clubs', singular: 'Club' },
    ru: { plural: 'Клубы', singular: 'Клуб' }
  });

  clubs.addField('title', 'Название', { isTitleField: true })
    .setType('string')
    .setRequired();

  clubs.addLinkField('managers', 'createdByManagerId', 'Создано менеджером')
    .setRequired()
    .setNotUpdatableByUser(undefined, 'ctx.service(\'profile\').getManagerId()');

  clubs.addLinkField('managers', 'lastChangedByManagerId', 'Изменено менеджером')
    .setNotUpdatableByUser(undefined);

  return clubs;
};
```

### Отчеты (Reports)

```typescript
const addReports = (system: SystemMetaBuilder) => {
  const reports = system.addCatalog('reportForParents');
  reports.setNeedFor('Таблица отчетов для родителей');
  reports.setTitles({
    en: { plural: 'Reports for parents', singular: 'Report for parent' },
    ru: { plural: 'Отчеты для родителей', singular: 'Отчет для родителей' }
  });

  // Основные поля
  reports.addField('title', 'Название')
    .setType('string')
    .setRequired();

  reports.addField('lastUpdated', 'Дата последнего изменения')
    .setType('date')
    .setNotUpdatableByUser(undefined, 'new Date()');

  reports.addField('paid', 'Оплачен')
    .setType('bool');

  // Связи
  reports.addLinkField('players', 'playerId', 'Игрок')
    .setRequired();

  reports.addLinkField('matches', 'matchId', 'Матч')
    .setRequired();

  reports.addLinkField('parents', 'parentId', 'Родитель')
    .setRequired();

  return reports;
};
```

## Примеры конфигурации

### Настройка меню

```typescript
// src/meta/addMenu.ts
import { SystemMetaBuilder } from 'runlify';

const addMenu = (system: SystemMetaBuilder) => {
  system.addMenuItem('main', {
    items: [
      {
        title: 'Команды',
        icon: 'Group',
        items: [
          { title: 'Список команд', link: '/teams' },
          { title: 'Создать команду', link: '/teams/create' }
        ]
      },
      {
        title: 'Клубы',
        icon: 'Business',
        items: [
          { title: 'Список клубов', link: '/clubs' },
          { title: 'Создать клуб', link: '/clubs/create' }
        ]
      },
      {
        title: 'Отчеты',
        icon: 'Assessment',
        items: [
          { title: 'Отчеты для родителей', link: '/reports/parents' },
          { title: 'Отчеты для клубов', link: '/reports/clubs' }
        ]
      }
    ]
  });
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
  entity.setSearchEnabled(true);
  entity.setAuditable(true);

  // Кастомные права
  entity.addPermission('canApprove', 'approve');
  entity.addPermission('canReject', 'reject');
};
```

## Примеры кастомных методов

### Одобрение отчета

```typescript
const reports = system.addCatalog('reports');

reports.addMethod('approve', 'update', 'Одобрить отчет')
  .setNeedFor('Метод для одобрения отчета')
  .addParameter('comment', 'Комментарий')
  .setType('string');

// Реализация в src/rest/reports/approve.ts
export const approveReport = async (ctx: Context) => {
  const { id, comment } = ctx.request.body;
  
  await ctx.prisma.reports.update({
    where: { id },
    data: {
      status: 'APPROVED',
      approvedAt: new Date(),
      approvedBy: ctx.user.id,
      comment
    }
  });
  
  return { success: true };
};
```

### Генерация отчета

```typescript
const reports = system.addCatalog('reports');

reports.addMethod('generate', 'create', 'Сгенерировать отчет')
  .setNeedFor('Метод для генерации отчета')
  .addParameter('playerId', 'ID игрока')
  .setType('string')
  .setRequired()
  .addParameter('period', 'Период')
  .setType('object')
  .setRequired();

// Реализация в src/rest/reports/generate.ts
export const generateReport = async (ctx: Context) => {
  const { playerId, period } = ctx.request.body;
  
  const data = await collectPlayerData(playerId, period);
  const report = await generateReportFromTemplate(data);
  
  return report;
};
```

## Примеры интеграции с frontend

### Компонент списка команд

```typescript
// src/components/TeamsList.tsx
import { useTeamsList } from '../generated/hooks';

export const TeamsList = () => {
  const { data, loading } = useTeamsList({
    filter: {
      clubId: currentClubId
    },
    sort: {
      field: 'title',
      order: 'ASC'
    }
  });

  if (loading) return <Loader />;

  return (
    <Table>
      <TableHead>
        <TableRow>
          <TableCell>Название</TableCell>
          <TableCell>Клуб</TableCell>
          <TableCell>Возраст с</TableCell>
          <TableCell>Возраст по</TableCell>
          <TableCell>Действия</TableCell>
        </TableRow>
      </TableHead>
      <TableBody>
        {data.map(team => (
          <TableRow key={team.id}>
            <TableCell>{team.title}</TableCell>
            <TableCell>{team.club.title}</TableCell>
            <TableCell>{team.dateOfBirthFrom}</TableCell>
            <TableCell>{team.dateOfBirthTo}</TableCell>
            <TableCell>
              <IconButton onClick={() => handleEdit(team.id)}>
                <EditIcon />
              </IconButton>
            </TableCell>
          </TableRow>
        ))}
      </TableBody>
    </Table>
  );
};
```

### Форма создания отчета

```typescript
// src/components/ReportForm.tsx
import { useCreateReport } from '../generated/hooks';

export const ReportForm = () => {
  const [createReport] = useCreateReport();

  const handleSubmit = async (values) => {
    try {
      await createReport({
        variables: {
          input: {
            title: values.title,
            playerId: values.playerId,
            matchId: values.matchId,
            parentId: values.parentId
          }
        }
      });
      
      showSuccess('Отчет создан');
    } catch (error) {
      showError('Ошибка при создании отчета');
    }
  };

  return (
    <Form onSubmit={handleSubmit}>
      <TextField name="title" label="Название" required />
      <PlayerSelect name="playerId" label="Игрок" required />
      <MatchSelect name="matchId" label="Матч" required />
      <ParentSelect name="parentId" label="Родитель" required />
      <Button type="submit">Создать</Button>
    </Form>
  );
};
```

## Следующие шаги

- Изучите [лучшие практики](./08-best-practices.md) для эффективной работы с Runlify
- Ознакомьтесь с разделом [Troubleshooting](./09-troubleshooting.md) для решения типичных проблем
- Посмотрите [API Reference](./06-api-reference.md) для подробного описания всех доступных методов 