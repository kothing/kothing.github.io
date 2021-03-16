---
layout: post
title:  "React createContext的TypeScript写法"
author: Kothing
categories: [ Typescript, React ]
image: assets/images/14.jpg
rating: 4.5
---

type.tsx
```typescript
export enum ItemType {
  Reveal = 'REVEAL',
  Replay = 'REPLAY'
}

export enum ViewMode {
  View,
  Edit
}
```

context.tsx
```typescript
import React, { createContext, Dispatch, SetStateAction, } from 'react';
import { FormInstance } from 'antd';
import { ReportType, ViewMode } from './types';

export const ItemContext = createContext<{
  caseId: number;
  reportId: number;
  type: ItemType;
  form: FormInstance<any>;
  viewMode: ViewMode;
  setViewMode: Dispatch<SetStateAction<ViewMode>>;
  forceUpdate: () => void;
}>({} as any);
```
