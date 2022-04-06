# Работа со строками и массивами

Фунции split, flat и trim для массива строк:

```typescript
const split = (a: string, s: string): string[] => a.split(s)
const trim = (a: string): string => a.trim()
const flat = (array: string[][]): string[] => ([] as string[]).concat(...array)
```

flat (создание плоского массива из из вложенных массивов)

```typescript
const flat = (array: SomeType[][]): SomeType[] => ([] as SomeType[]).concat(...array)
```
