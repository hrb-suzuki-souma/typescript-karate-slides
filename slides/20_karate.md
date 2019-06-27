# TypeScript空手

- 型パズルを駆使して複雑なプロダクトに備えること
- または倒してしまうこと
- 今日はちょっとだけ「Kata」をご紹介

---

# Union Types
A だったり B だったりする型
```ts
type Union = A | B

```

---

# こんな事できます
`true` のときだけパラメータが増えるStruct
```ts
type WhenTrue = { flag: true, text: string }
type WhenFalse = { flag: false }
type Params = WhenTrue | WhenFalse

function textIntoConsole(params: Params): void {
  //              ↓Type Guard
  if (params.flag === true) {
    console.log(params.text)
  }

  console.log(params.text)
  //                 ^^^^ コンパイルエラー
}
```

---

# keyof
連想配列のキーを列挙
```ts
type Hash = {
  name: string
  age: number
}
type Key = keyof Hash // 'name' | 'age'
```

---

# Conditional Types
型の条件に応じて違う型を返す
```ts
type TypeName<T> =
  T extends string ? : 'string' :
  T extends number ? : 'number' :
  never

type StringName = TypeName<string> // 'string'
type NumberName = TypeName<number> // 'number'
```

---

# こんな事できます
Union Types の Diff
```ts
type Diff<T, U> = T extends U ? never : T

type Sushi = 'maguro' | 'ika' | 'aji' | 'gari'
type TrueSushi = Diff<Sushi, 'gari'> // 'maguro' | 'ika' | 'aji'
```

---

# 型境界
型変数に数字だけ受け入れる
```ts
class Hour<T extends number> {
  constructor(private value: T) {}

  getValue(): T {
    return this.value
  }
}
```

---

# こんな事できます
実在する値しか受け入れない `Hour` クラス
```ts
type HourValue = 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12

class Hour<T extends HourValue> {
  constructor(private value: T) {}

  getValue(): T {
    return this.value
  }
}

const one = new Hour(1)
const thirteen = new Hour(13)
//                        ^^ コンパイルエラー

```

---

# 他にもたくさんある…
世界がまだ見ぬ型があるかもしれない…！君だけの型を作ろう…！

---

# 一例
APIの型定義
```ts
type SuccessCode = 200 | 204
type ErrorCode = 400 | 401 | 500 | 503
type APIResponse<T> =
  {
    code: SuccessCode
    data: T
  }
  | {
    code: ErrorCode
    message: string
  }

type Sushi = {
    name: string
  } & 
  ({
    sumeshi: false
  }
  | {
    sumeshi: true
    neta: string
  })

function getSushi(): Sushi {
  const response: APIResponse<Sushi> = requestAPI('/api/sushi')

  if (isErrorCode(reponse.code)) throw new Error('API request is failed')
  console.info(response.message)

  return response.data
}

const sushi = getSushi()

if (sushi.sumeshi) {
  console.log(`${sushi.name} is ${sushi.neta} on Shari`)
} else {
  console.log(`${sushi.name} is Sashimi yanke`)
}

```

---

# 空手で培ったAPIの型定義をお待ちしています 😊


