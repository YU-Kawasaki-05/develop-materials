# 0313 TypeScript詳細

## 🎯 この章で学ぶこと
- TypeScriptの静的型付けがなぜ重要で、どのような問題を解決するのかを理解する。
- `string`や`number`のような基本の型から、`Array`, `Object`, `Tuple`, `Enum`などの複雑な型定義の方法を習得する。
- `any`, `unknown`, `void`, `never`といった特殊な型の役割と使い分けを学ぶ。
- ジェネリクス（Generics）を用いて、再利用可能で型安全なコンポーネントを作成する方法を理解する。
- `interface`と`type`エイリアスの違いを理解し、適切な場面で使い分けられるようになる。

## 🤔 なぜ重要なのか
JavaScriptは動的型付け言語であり、柔軟性が高い反面、大規模な開発やチームでの開発では予期せぬエラーを生み出す原因となりがちです。「実行して初めてエラーに気づく」という状況は、開発効率を著しく低下させます。

TypeScriptは、この問題に対する強力な解決策です。コードを書いている段階で型に関するエラーを検出できるため、バグの早期発見、コードの可読性向上、そしてエディタの強力な補完機能（インテリセンス）による開発体験の向上に直結します。現代のフロントエンド開発やNode.jsを用いたサーバーサイド開発において、TypeScriptは堅牢なアプリケーションを構築するためのデファクトスタンダード（事実上の標準）となりつつあります。

## 📚 基礎概念の理解

### 静的型付けと基本的な型
JavaScriptでは、変数の型は実行時に決まります。
```javascript
let message = "こんにちは"; // messageはstring型
message = 123; // エラーにならず、number型に変わる
```
一方、TypeScriptでは、コードを書く段階で変数の型を宣言します。これを**静的型付け（Static Typing）**と呼びます。

```typescript
let message: string = "こんにちは";
// message = 123; // ここでコンパイルエラー！「Type 'number' is not assignable to type 'string'.」
```
このおかげで、意図しない型が代入されることを防げます。

**基本的な型**:
- `string`: 文字列 (`"hello"`, `'world'`)
- `number`: 数値 (`10`, `3.14`)
- `boolean`: 真偽値 (`true`, `false`)
- `null`: `null`値
- `undefined`: `undefined`値
- `symbol`: 一意なシンボル値
- `bigint`: 巨大な整数

### 複雑な型：Array, Tuple, Enum, Object
- **Array（配列）**: 同じ型の要素の集合。`string[]`や`Array<number>`のように記述します。
  ```typescript
  const list: number[] = [1, 2, 3];
  ```
- **Tuple（タプル）**: 固定数の要素を持ち、各要素の型が決まっている配列。
  ```typescript
  let user: [number, string] = [1, "Alice"];
  // user = ["Bob", 2]; // エラー！型の順序が違う
  ```
- **Enum（列挙型）**: 特定のグループに属する定数に名前を付ける方法。
  ```typescript
  enum Color {
    Red,    // 0
    Green,  // 1
    Blue    // 2
  }
  let c: Color = Color.Green; // 1
  ```
- **Object（オブジェクト）**: 最も一般的な型。プロパティと型を定義できます。
  ```typescript
  let person: { name: string; age: number } = {
    name: "Bob",
    age: 30,
  };
  ```

### 特殊な型：any, unknown, void, never
- **any**: 何でもありの型。型チェックを無効にするため、最終手段として使用します。
- **unknown**: `any`と似ていますが、より安全です。`unknown`型の変数を使用するには、型チェック（typeofやinstanceofなど）を行って型を確定させる必要があります。
- **void**: 関数が何も返さないことを示す型。
  ```typescript
  function sayHello(): void {
    console.log("Hello!");
  }
  ```
- **never**: 関数が決して戻り値を返さない（常にエラーをスローするか、無限ループに陥る）ことを示す型。

## 💡 実践的な活用

### ハンズオン：型安全なユーザー情報管理関数
ユーザーオブジェクトを受け取り、その情報を表示する関数を作成してみましょう。

**JavaScriptの場合（問題点）**:
```javascript
function printUser(user) {
  // user.name や user.age が存在するかは実行時までわからない
  // user.agee のようなタイポもエラーにならない
  console.log(`Name: ${user.name}, Age: ${user.age}`);
}
```

**TypeScriptの場合（改善）**:
1. `interface`または`type`でユーザーの型を定義します。
   ```typescript
   interface User {
     id: number;
     name: string;
     age: number;
     email?: string; // `?` はオプショナル（任意）なプロパティを示す
   }
   ```
2. その型を関数の引数に適用します。
   ```typescript
   function printUser(user: User): void {
     console.log(`ID: ${user.id}, Name: ${user.name}, Age: ${user.age}`);
     if (user.email) {
       console.log(`Email: ${user.email}`);
     }
   }
   ```
**期待される結果**:
- `printUser`関数を呼び出す際、`User`インターフェースに適合しないオブジェクト（例：`age`がない）を渡そうとすると、コンパイル時にエラーで教えてくれます。
- 関数内で`user.naem`のようなタイポをすると、エディタが即座にエラーを指摘します。

### よくある落とし穴
- **`any`の乱用**: `any`を使いすぎると、TypeScriptの利点である型安全性が失われます。できるだけ具体的な型を定義し、不明な場合は`unknown`を使うことを検討しましょう。
- **型の推論に頼りすぎ**: TypeScriptは初期値から型を推論してくれますが、関数の戻り値や複雑なオブジェクトなど、明示的に型を定義した方が意図が明確になり、安全性が高まる場面も多いです。

## 🔍 深掘り：プロの視点

### ジェネリクス（Generics）で再利用性を高める
ジェネリクスは、特定の型に縛られず、様々な型で動作する関数やクラスを作成するための機能です。`T`のような型変数を使い、コンポーネントが使用されるときに具体的な型が決定されます。

**例：何でも受け取れる配列を返す関数**
```typescript
// Tは型引数。関数が呼び出されるときに型が決まる
function identity<T>(arg: T): T {
  return arg;
}

let output1 = identity<string>("myString"); // Tはstring
let output2 = identity<number>(123);      // Tはnumber
```
これにより、`identity`関数は`string`専用や`number`専用の関数をそれぞれ作る必要がなく、型安全性を保ちながら再利用できます。APIのレスポンスを扱うラッパーや、データ構造を実装する際などに非常に強力です。

### `interface` vs `type`エイリアス
`interface`と`type`はどちらもオブジェクトの型を定義できますが、いくつかの重要な違いがあります。

| 特徴 | `interface` | `type` |
| :--- | :--- | :--- |
| **拡張** | `extends`キーワードで拡張可能 | `&`（交差型）で拡張可能 |
| **同名宣言** | 複数回宣言すると自動でマージされる | 複数回宣言するとエラーになる |
| **用途** | オブジェクトの形状を定義するのに適している | プリミティブ型、合併型、タプルなど、より複雑な型定義も可能 |

**判断基準**:
- **APIの形状（オブジェクト）を定義する場合**: `interface`を推奨。拡張やマージの挙動が、オブジェクト指向の概念と親和性が高いため。
- **より複雑な型や、特定の方に別名をつけたい場合**: `type`を使用。例えば`type UserID = string | number;`のように、合併型を定義するのに便利。

## 📋 まとめとチェックポイント
- TypeScriptはJavaScriptに静的な型付けを加えたスーパーセットである。
- 型を定義することで、コンパイル時にエラーを発見し、コードの堅牢性と開発効率を高める。
- `string`, `number`などの基本の型から、`Array`, `Tuple`, `Object`などの複雑な型、`any`, `unknown`などの特殊な型を理解した。
- ジェネリクスを使うと、型安全で再利用可能なコードを書くことができる。
- `interface`と`type`は似ているが、拡張性や宣言のマージ挙動に違いがあり、用途に応じて使い分ける。

**チェックポイント**:
- なぜ「`any`ではなく`unknown`を使うべき」と言われるのか、その理由を説明できますか？
- `interface`と`type`のどちらを使ってユーザープロファイルを定義しますか？その理由は？
- ジェネリクスを使わずにAPIレスポンスを処理する場合、どのような問題が考えられますか？

## 🔗 関連知識・発展学習
- [0122_Object_Oriented_Programming.md](./../012_Programming_Concepts/0122_Object_Oriented_Programming.md): `interface`の概念はオブジェクト指向プログラミングと深く関連しています。
- [0314_Module_System.md](./0314_Module_System.md): TypeScriptの型定義をモジュールとしてエクスポート・インポートする方法。
- **公式ドキュメント**: [TypeScript for JavaScript Programmers](https://www.typescriptlang.org/docs/handbook/typescript-for-javascript-programmers.html) 