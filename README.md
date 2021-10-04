# React * TypeSctipe

`npx create-react-app <name> --template typescript`

# TypeScriptの基本

## 型

公式... `変数: 型`

- string
- number
- boolean
- null
- undefined
- any ...これはどの型でもいいという意味になるためtypescriptを使っている意味がなくなるので極力使わないように

## Generics ジェネリクス

`const arr :Array<number> = [0,1,2];`

`<>`内に型を指定する方法をGenericsと呼ぶ

## 関数の型指定

関数の型は`引数の型`, `返却値の型`を指定できる

公式... `(引数: 引数の型): 返却値の型 => {}`

`void`...関数が何も返却しないことを意味する。つまり下記のように`return`していなければ`void`型である

```tsx
const funcA = (num: number): void => {
  console.log(num)
}
```

## tsconfig.json

既存のアプリをTypeScript化する際に`"strict: true"`にしておくとエラーが大量発生するので最初はfalseから始めるのがベター

## APIで取得するデータへの型宣言

```tsx
import { useEffect, useState } from 'react'
import { ListItem } from './components/ListItem'
import axios from 'axios';

type User = {
  id: number;
  name: string;
  age: number;
  personalColor: string;
}

export const App = () => {
  // 初期値：空の配列であり、Userで定義した型である配列と宣言
  const [users, setUsers] = useState<User[]>([])

  useEffect(() => {
    // type Userの型であるデータを取得するという意味
    axios.get<User[]>('https://example.com/users')
    .then(res => {
      setUsers(res.data)
    })
    // 空の配列を依存変数に指定することで初回マウント時のみ実行させる
  }, [])

  return (
    <div>
      {users.map((user) => (
        <ListItem id={user.id} name={user.name} age={user.age} />
      ))}
    </div>
  )
}
```

## Propsへの型定義

ListItemコンポーネントが受け取るpropsの型を定義する

```tsx
type User = {
  id: number;
  name: string;
  age: number
}

export const ListItem = (props: User) => {
  const { id, name, age } = props;
  return (
    <p>
      {id}: {name}({age})
    </p>
  )
}
```

<br>

## 関数コンポーネントの型定義

```tsx
export const ListItem :VFC = () => {
  //
}
```

`children: React.ReactNode`...childrenを使用する際は明示的に型を指定する
