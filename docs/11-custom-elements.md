# Кастомные элементы

В этом разделе описаны способы создания и использования кастомных элементов в Runlify.

## Виджеты

### Создание базового виджета

```typescript
// src/widgets/CustomWidget.tsx
import { FieldProps } from 'react-admin';
import { TextField } from '@mui/material';

export const CustomWidget = (props: FieldProps) => {
  const { source, record } = props;
  
  return (
    <TextField
      label={source}
      value={record?.[source]}
      fullWidth
    />
  );
};
```

### Виджеты для специальных типов данных

#### JSON виджет
```typescript
// src/widgets/JsonFieldWidget.tsx
import { FieldProps } from 'react-admin';
import { TextField } from '@mui/material';

export const JsonFieldWidget = (props: FieldProps) => {
  const { source, record } = props;
  
  return (
    <TextField
      label={source}
      value={JSON.stringify(record?.[source], null, 2)}
      multiline
      rows={4}
      fullWidth
    />
  );
};
```

#### Виджет для связанных сущностей
```typescript
// src/widgets/LinkedEntitiesWidget.tsx
import { FieldProps } from 'react-admin';
import { ReferenceField } from 'react-admin';

export const LinkedEntitiesWidget = (props: FieldProps) => {
  const { source, record, reference } = props;
  
  return (
    <ReferenceField
      source={source}
      reference={reference}
      link="show"
    >
      <TextField source="name" />
    </ReferenceField>
  );
};
```

### Графические виджеты

#### График области
```typescript
// src/widgets/AreaChartWidget.tsx
import { FieldProps } from 'react-admin';
import { AreaChart, Area, XAxis, YAxis, CartesianGrid, Tooltip } from 'recharts';

export const AreaChartWidget = (props: FieldProps) => {
  const { source, record } = props;
  const data = record?.[source] || [];

  return (
    <AreaChart width={600} height={400} data={data}>
      <CartesianGrid strokeDasharray="3 3" />
      <XAxis dataKey="name" />
      <YAxis />
      <Tooltip />
      <Area type="monotone" dataKey="value" stroke="#8884d8" fill="#8884d8" />
    </AreaChart>
  );
};
```

#### Столбчатый график
```typescript
// src/widgets/BarChartWidget.tsx
import { FieldProps } from 'react-admin';
import { BarChart, Bar, XAxis, YAxis, CartesianGrid, Tooltip } from 'recharts';

export const BarChartWidget = (props: FieldProps) => {
  const { source, record } = props;
  const data = record?.[source] || [];

  return (
    <BarChart width={600} height={400} data={data}>
      <CartesianGrid strokeDasharray="3 3" />
      <XAxis dataKey="name" />
      <YAxis />
      <Tooltip />
      <Bar dataKey="value" fill="#8884d8" />
    </BarChart>
  );
};
```

## UI компоненты

### Кнопки

#### Кнопка с иконкой
```typescript
// src/uiLib/IconButtonModal.tsx
import { IconButton, Tooltip } from '@mui/material';
import { Edit } from '@mui/icons-material';

export const IconButtonModal = ({ onClick, title }: { onClick: () => void, title: string }) => {
  return (
    <Tooltip title={title}>
      <IconButton onClick={onClick}>
        <Edit />
      </IconButton>
    </Tooltip>
  );
};
```

#### Кнопка с модальным окном
```typescript
// src/uiLib/ButtonModal.tsx
import { Button, Dialog, DialogTitle, DialogContent } from '@mui/material';

export const ButtonModal = ({ 
  title, 
  children, 
  open, 
  onClose 
}: { 
  title: string;
  children: React.ReactNode;
  open: boolean;
  onClose: () => void;
}) => {
  return (
    <Dialog open={open} onClose={onClose}>
      <DialogTitle>{title}</DialogTitle>
      <DialogContent>
        {children}
      </DialogContent>
    </Dialog>
  );
};
```

### Поля ввода

#### Поле даты
```typescript
// src/uiLib/DateField.tsx
import { DateField as RaDateField } from 'react-admin';

export const DateField = (props: any) => {
  return (
    <RaDateField
      {...props}
      locales="ru-RU"
      showTime={false}
    />
  );
};
```

#### Поле с поддержкой Markdown
```typescript
// src/uiLib/ReactMarkdownField.tsx
import { FieldProps } from 'react-admin';
import ReactMarkdown from 'react-markdown';

export const ReactMarkdownField = (props: FieldProps) => {
  const { source, record } = props;
  
  return (
    <ReactMarkdown>
      {record?.[source] || ''}
    </ReactMarkdown>
  );
};
```

### Компоненты отображения данных

#### Таблица с сортировкой
```typescript
// src/uiLib/DataGridWidth.tsx
import { DataGrid } from '@mui/x-data-grid';

export const DataGridWidth = ({ 
  columns, 
  rows 
}: { 
  columns: any[];
  rows: any[];
}) => {
  return (
    <DataGrid
      columns={columns}
      rows={rows}
      autoHeight
      disableSelectionOnClick
    />
  );
};
```

#### Фильтры с сортировкой
```typescript
// src/uiLib/SortedFilters.tsx
import { Filter, TextInput } from 'react-admin';

export const SortedFilters = [
  <Filter key="name">
    <TextInput source="name" label="Имя" />
  </Filter>
];
```

## Использование кастомных элементов

### В метаданных

```typescript
// src/meta/addCatalogs.ts
const team = system.addCatalog('team')
  .setTitle('Команды')
  .addScalarField('name', 'string')
  .setTitleField('name')
  .setRequired('name')
  .addScalarField('description', 'string')
  .setWidget('description', 'ReactMarkdownField')
  .addScalarField('stats', 'json')
  .setWidget('stats', 'AreaChartWidget')
  .enableSearch()
  .enableAudit();
```

### В React Admin

```typescript
// src/components/TeamList.tsx
import { List, Datagrid, TextField } from 'react-admin';
import { CustomWidget } from '../widgets/CustomWidget';

export const TeamList = () => (
  <List>
    <Datagrid>
      <TextField source="name" />
      <CustomWidget source="customField" />
    </Datagrid>
  </List>
);
```

## Рекомендации по созданию кастомных элементов

1. **Структура**
   - Размещайте виджеты в `src/widgets/`
   - Размещайте UI компоненты в `src/uiLib/`
   - Используйте TypeScript для типизации

2. **Стилизация**
   - Используйте Material-UI компоненты
   - Следуйте единому стилю оформления
   - Поддерживайте темную тему

3. **Производительность**
   - Оптимизируйте рендеринг
   - Используйте мемоизацию где необходимо
   - Минимизируйте количество ререндеров

4. **Доступность**
   - Добавляйте ARIA-атрибуты
   - Обеспечивайте навигацию с клавиатуры
   - Поддерживайте режим высокой контрастности

## Следующие шаги

- Ознакомьтесь с лучшими практиками в разделе [Лучшие практики](08-best-practices.md)
- Изучите примеры использования в разделе [Примеры](07-examples.md)
- При возникновении проблем обратитесь к разделу [Устранение неполадок](09-troubleshooting.md) 