# 유틸 타입

## Identical
```typescript
export type Identical<key extends string, value extends any> = { [k in key]: value };

// @example 
const { username, password } = req.body as Identical<'username' | 'password', string>;
```
