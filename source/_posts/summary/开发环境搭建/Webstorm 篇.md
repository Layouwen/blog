---
title: Webstorm 篇
date: 2024-01-28T15:22:00.000Z
tags:
  - 汇总
  - webstorm
categories:
  - 汇总
  - 开发环境
uuid: 44cba0ae-30b4-4aec-9ce3-edbbfb48f87d
---

# Live Templates

## React

禁用 `con`
禁用 `fsc`
禁用 `fsf`
禁用 `props`
禁用 `rcc`
禁用 `rccp`
禁用 `rcfc`
禁用 `rsc`
禁用 `rscp`
禁用 `rsf`
禁用 `rsfp`
禁用 `rsi`
禁用 `state`

rfch - 默认导出 const 函数组件

## JavaScript

arf - const 箭头函数

Abbreviation: `edaf`
Description: `export { default as $COMPONENT_NAME$ } from "./$COMPONENT_NAME$.tsx";`
scope: `JavaScript/Top level statement` & `TypeScript/Top level statement`

```ts
export { default as $COMPONENT_NAME$ } from "./$COMPONENT_NAME$.tsx";
```

## Zustand

Abbreviation: `zus`
Description: `zustand useStore 模板`
scope: `JavaScript/Top level statement` & `TypeScript/Top level statement`

```ts
import { create } from 'zustand';

type State = {
  $KEY$: $KEY_TYPE$;
};

type Actions = {
  set$KEY_CASE$: (d: $KEY_TYPE$) => void;
};

export const use$STORE_NAME$Store = create<State & Actions>((set, get) => ({
  $KEY$: '',
  set$KEY_CASE$: data => {
    console.log(get().$KEY$)
    set({ $KEY$: data })
  }
}));
```

## @tanstack/react-query

Abbreviation: `tuq`
Description: `useQuery 模板`
scope: `JavaScript/Top level statement` & `TypeScript/Top level statement`

| Name          | Expression           | Default value | Skip if defined |
| ------------- | -------------------- | ------------- | --------------- |
| API_NAME      |                      |               |                 |
| API_NAME_CASE | capitalize(API_NAME) |               | x               |

```ts
import { useQuery } from '@tanstack/react-query';
import { useMemo } from 'react';
import type { $API_NAME_CASE$ApiParams } from '@/api'
import { $API_NAME$Api } from '@/api';
import { isSuccessApi } from '@/utils';

export const use$API_NAME_CASE$QueryQueryKey = 'use$API_NAME_CASE$Query';

export function use$API_NAME_CASE$Query(options?: {
  params?: $API_NAME_CASE$ApiParams;
  options?: {
    enabled?: boolean;
  };
}) {
  const { data: response, ...rest } = useQuery({
    queryFn: ({ queryKey }) => $API_NAME$Api(queryKey[1]),
    queryKey: [use$API_NAME_CASE$QueryQueryKey, options?.params] as const,
    ...options?.options,
  });

  const data = useMemo(() => {
    if (!isSuccessApi(response)) return;
    return response.data;
  }, [response]);

  return {
    data,
    response,
    ...rest,
  };
};
```

Abbreviation: `tum`
Description: `useMutation 模板`
scope: `JavaScript/Top level statement` & `TypeScript/Top level statement`

| Name              | Expression               | Default value | Skip if defined |
| ----------------- | ------------------------ | ------------- | --------------- |
| API_NAME          |                          |               |                 |
| API_NAME_CASE     | capitalize(API_NAME)     |               | x               |
| GET_API_NAME      |                          |               |                 |
| GET_API_NAME_CASE | capitalize(GET_API_NAME) |               | x               |


```ts
import { useMutation } from '@tanstack/react-query';
import { $API_NAME$Api } from '@/api';
import { queryClient } from '@/main';
import { useGet$GET_API_NAME$QueryQueryKey } from '@/hooks/useGet$GET_API_NAME_CASE$Query';

export const use$API_NAME_CASE$Mutation = () => {
  const { mutateAsync, ...rest } = useMutation({
    mutationFn: $API_NAME$Api,
    onSuccess: () => {
      void queryClient.invalidateQueries({
        queryKey: [useGet$GET_API_NAME_CASE$QueryQueryKey],
      });
    },
  });

  return [
    mutateAsync,
    {
      ...rest,
    },
  ] as const;
};
```
