---
layout: post
title:  "React createContext的TypeScript写法"
author: Kothing
categories: [ Javascript, React ]
image: assets/images/14.jpg
rating: 4.5
---

type.ts
```
export enum RiskReportType {
  Reveal = 'REVEAL',
  Replay = 'REPLAY'
}

export enum ViewMode {
  View,
  Edit
}
```

```typescript
import React, { Dispatch, SetStateAction, } from '@alipay/bigfish/react';
import { RiskReportType, RiskReportItemType, RiskReportStatus, RiskReportItemStatus, RiskReportItemResult } from './types';
import { FormInstance } from 'antd';
import { ViewMode } from './types';

export const RiskReportContext = React.createContext<{
  caseId: number;
  reportId: number;
  type: RiskReportType;
  form: FormInstance<any>;
  viewMode: ViewMode;
  setViewMode: Dispatch<SetStateAction<ViewMode>>;
  forceUpdate: () => void;
}>({} as any);
```
