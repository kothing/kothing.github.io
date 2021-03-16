---
layout: post
title:  "React使用createContext的TypeScript写法"
author: Kothing
categories: [ TypeScript, React ]
tags: [ TypeScript ]
image: assets/images/14.jpg
rating: 4.5
---

## React createContext的TypeScript写法

React中使用createContext的TypeScript写法示例


```typescript
// type.tsx
export enum ItemType {
  Reveal = 'REVEAL',
  Replay = 'REPLAY'
}

export enum ViewMode {
  View,
  Edit
}

export enum ItemStatus {
  Draft = 'DRAFT',
  Approval = 'APPROVAL',
  Approved = 'APPROVED',
  Done = 'DONE'
}
```

```typescript
// constant.ts
import { ItemStatus } from './types';
export const ItemStatusConf: Record<ItemStatus, string> = {
  [ItemStatus.Draft]: '草稿',
  [ItemStatus.Approval]: '待审核',
  [ItemStatus.Approved]: '已审核',
  [ItemStatus.Done]: '已完成'
}
```


```typescript
// context.tsx
import React, { createContext, Dispatch, SetStateAction, } from 'react';
import { FormInstance } from 'antd';
import { ItemType, ViewMode } from './types';

export const ItemContext = createContext<{
  id: number;
  type: ItemType;
  form: FormInstance<any>;
  status?: ItemStatus;
  viewMode: ViewMode;
  setViewMode: Dispatch<SetStateAction<ViewMode>>;
  forceUpdate?: () => void;
}>({} as any);
```

```typescript
// Child.tsx

import React, { useContext } from 'react';
import { Form, Radio } from 'antd';
import { ItemStatus } from './types';
import { ItemStatusConf } from './constant';
import { ItemContext } from './context.ts';

interface ChildProps {
}

const Child: React.FC<ChildProps> = () => {
  const { id, type, form, viewMode, setViewMode } = useContext(ItemContext);

  const [value, setValue] = useState<ItemStatus>(ItemStatus.Draft);

  const onChange = e => {
    console.log('radio checked', e.target.value);
    setValue(e.target.value);
  };

  return (
    <Form
      name="test_form"
      form={form}
    >
      <Form.Item name="status" label="状态">
        <Radio.Group onChange={onChange} value={value}>
          {Object.entries(ItemStatusConf).map(([status, text]) => (<Radio value={status}>{text}</Radio>))}
        </Radio.Group>
      </Form.Item>
    </Form>
  );
};

export default Child;
```

```typescript
// App.tsx

import React, { useMemo } from 'react';
import { Form } from 'antd';
import { ItemType, ViewMode } from './types';
import { ItemStatusConf } from './constant';
import { ItemContext } from './context.ts';
import Child from './Child';

interface AppProps {
  id: number;
  type: ItemType;
}

const forceUpdate = () => {
  console.log('forceUpdate');
}

const App: React.FC<AppProps> = ({ id, type }) => {
  const initMode = !id ? ViewMode.Edit : ViewMode.View;
  const [viewMode, setViewMode] = useState<ViewMode>(initMode);
  const [form] = Form.useForm();

  const contextValue = useMemo(() => ({
    id,
    type: ItemType.Reveal,
    form,
    viewMode, 
    setViewMode,
    forceUpdate
  }), [id, type, viewMode, setViewMode, forceUpdate]);

  return (
    <ItemContext.Provider value={contextValue}>
      <Child/>
    </ItemContext.Provider>
  );
};

export default App;
```
