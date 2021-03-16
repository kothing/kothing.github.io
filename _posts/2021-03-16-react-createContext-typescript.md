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
  [ItemStatus.Approval]: '报告审核',
  [ItemStatus.Approved]: '审核通过',
  [ItemStatus.Done]: '已完成'
}
```


```typescript
// context.tsx
import React, { createContext, Dispatch, SetStateAction, } from 'react';
import { FormInstance } from 'antd';
import { ReportType, ViewMode } from './types';

export const ItemContext = createContext<{
  id: number;
  type: ItemType;
  form?: FormInstance<any>;
  status?: ItemStatus;
  viewMode: ViewMode;
  setViewMode: Dispatch<SetStateAction<ViewMode>>;
  forceUpdate?: () => void;
}>({} as any);
```

```typescript
// App.tsx
import { Radio } from 'antd';
import { ItemType, ItemStatus, ViewMode } from './types';
import { ItemContext } from './context.ts'
import { ItemStatusConf } from './constant';

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
  const [radioValue, setRadioValue] = useState<ItemStatus>(ItemStatus.Draft);

  const onChange = e => {
    console.log('radio checked', e.target.value);
    setValue(e.target.value);
  };

  const contextValue = useMemo(() => ({
    id,
    type,
    viewMode, 
    setViewMode,
    forceUpdate
  }), [id, type, viewMode, setViewMode, forceUpdate]);

  return (
    <RiskReportContext.Provider value={contextValue}>
      <Radio.Group onChange={onChange} value={radioValue}>
        {Object.entries(ItemStatusConf).map(([value, text]) => (<Radio value={value}>{text}</Radio>))}
      </Radio.Group>
    </RiskReportContext.Provider>
  );
};
```
