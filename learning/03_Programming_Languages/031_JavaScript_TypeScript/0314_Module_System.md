# 0314 モジュールシステム

## 🎯 この章で学ぶこと
- なぜコードをモジュールに分割する必要があるのか、その動機と利点を理解する。
- JavaScriptの標準モジュールシステムである「ESM（ECMAScript Modules）」の`import`/`export`構文を習得する。
- 古くからNode.jsで使われてきた「CommonJS」モジュールシステムの`require`/`module.exports`構文を理解する。
- `export default`（デフォルトエクスポート）と`export`（名前付きエクスポート）の違いと、それぞれの使い分け方を学ぶ。
- TypeScriptにおけるモジュールの扱い方と、JavaScriptへのコンパイル時の挙動を理解する。

## 🤔 なぜ重要なのか
一つのファイルに数千行のコードを書いていくと、どうなるでしょうか？
- **可読性の低下**: どこに何が書かれているか把握するのが困難になります。
- **再利用性の欠如**: 特定の関数を別のプロジェクトで使いたい場合、コピペするしかありません。
- **依存関係の混乱**: グローバルスコープに多くの変数や関数が定義され、名前の衝突や意図しない上書きが発生しやすくなります（「名前空間の汚染」）。

モジュールシステムは、これらの問題を解決するために不可欠な仕組みです。コードを機能ごとに独立したファイル（モジュール）に分割し、必要なものだけを明確にインポート（読み込み）・エクスポート（公開）することで、大規模で複雑なアプリケーションでも秩序を保つことができます。これにより、コードは整理され、メンテナンスしやすく、再利用可能になります。

## 📚 基礎概念の理解

### モジュールとは？
モジュールとは、特定の機能や関心事ごとに分割されたコードの単位です。通常、1ファイルが1モジュールに対応します。モジュールは自身が持つ変数、関数、クラスなどを、デフォルトでは外部からアクセスできないようにカプセル化します。そして、`export`文を使って、外部に公開したいものだけを明示的に指定します。他のモジュールは`import`文を使って、公開された機能を利用します。

### ESM (ECMAScript Modules)
現代のJavaScriptにおける標準的なモジュールシステムです。ブラウザとNode.jsの両方でサポートされています。

**エクスポート (export)**
- **名前付きエクスポート (Named Export)**: 複数の機能をエクスポートできます。インポート時には同じ名前を使う必要があります。
  ```typescript
  // utils.ts
  export const PI = 3.14;
  export function double(n: number): number {
    return n * 2;
  }
  ```
- **デフォルトエクスポート (Default Export)**: 1モジュールにつき1つだけエクスポートできます。インポート時には好きな名前を付けられます。
  ```typescript
  // User.ts
  export default class User {
    // ...クラス定義
  }
  ```

**インポート (import)**
- **名前付きインポート**: `{}` を使って、特定のエクスポートをインポートします。
  ```typescript
  // main.ts
  import { PI, double } from './utils.ts';
  console.log(PI); // 3.14
  ```
- **デフォルトインポート**:
  ```typescript
  // main.ts
  import MyUser from './User.ts'; // 好きな名前（MyUser）でインポート
  const user = new MyUser();
  ```
- **すべてインポート**: `*` を使って、モジュールからエクスポートされたものすべてをまとめてインポートできます。
  ```typescript
  // main.ts
  import * as utils from './utils.ts';
  console.log(utils.PI);
  console.log(utils.double(2));
  ```

### CommonJS
主にNode.jsの初期から使われてきたモジュールシステムです。ESMが標準化される前のデファクトスタンダードでした。

- **エクスポート**: `module.exports` または `exports` オブジェクトに代入します。
  ```javascript
  // utils.js
  const PI = 3.14;
  function double(n) {
    return n * 2;
  }
  module.exports = { PI, double };
  ```
- **インポート**: `require()` 関数を使います。
  ```javascript
  // main.js
  const { PI, double } = require('./utils.js');
  console.log(PI);
  ```

**ESMとCommonJSの違い**: ESMは静的であり、コードを実行する前に`import`/`export`の関係を解析できます。これにより、不要なコードを削除する最適化（Tree Shaking）などが可能になります。一方、CommonJSは動的で、`require`はコードの実行中に評価されます。

## 💡 実践的な活用

### ハンズオン：簡単な電卓モジュール作成
1.  **`math.ts` を作成（名前付きエクスポート）**
    ```typescript
    // math.ts
    export const add = (a: number, b: number): number => a + b;
    export const subtract = (a: number, b: number): number => a - b;
    ```
2.  **`calculator.ts` を作成（デフォルトエクスポート）**
    ```typescript
    // calculator.ts
    import * as math from './math.ts';

    class Calculator {
      add(a: number, b: number): number {
        return math.add(a, b);
      }
      subtract(a: number, b: number): number {
        return math.subtract(a, b);
      }
    }
    export default Calculator;
    ```
3.  **`main.ts` で利用**
    ```typescript
    // main.ts
    import MyCalculator from './calculator.ts'; // デフォルトインポート
    import { add } from './math.ts'; // 名前付きインポートも可能

    const calc = new MyCalculator();
    console.log(calc.add(5, 3)); // 8
    console.log(add(10, 20)); // 30
    ```
この例では、`math.ts`が基本的な計算機能を提供し、`calculator.ts`がそれを内部で利用してより高レベルなクラスを提供しています。`main.ts`は両方のモジュールから必要な機能をインポートして利用します。これにより、各ファイルの関心事が明確に分離されました。

## 🔍 深掘り：プロの視点

### デフォルトエクスポート vs 名前付きエクスポート
この2つの使い分けは、しばしば議論の的になりますが、一般的なガイドラインは以下の通りです。

- **デフォルトエクスポート (`export default`) を使う場合**:
  - そのモジュールが「主役」となる単一のクラスや関数を提供するとき。（例: `React.Component`, `express()`）
  - インポート側で自由に名前を付けさせたいとき。

- **名前付きエクスポート (`export`) を使う場合**:
  - 複数の独立したユーティリティ関数や定数を提供するとき。（例: `lodash`の各関数）
  - インポート時に常に同じ名前で参照させたいとき。これにより、コードベース全体で名称が統一され、リファクタリング（特に名前の変更）が容易になります。

多くのプロジェクトでは、**名前付きエクスポートを基本とし、本当にそのモジュールの中心的な存在であるものだけをデフォルトエクスポートする**、という方針がとられています。

### TypeScriptとモジュール
TypeScriptのコード（`.ts`）は、最終的にJavaScript（`.js`）にコンパイルされます。TypeScriptコンパイラ（`tsc`）は、`tsconfig.json`の設定に基づいて、ESM形式のJavaScriptを生成するか、CommonJS形式のJavaScriptを生成するかを決定します。

- **`"module": "ESNext"`**: ESM形式で出力します。
- **`"module": "CommonJS"`**: CommonJS形式で出力します。

これにより、TypeScriptのコードは一度書けば、ターゲットとする実行環境（最新のブラウザや古いNode.js環境など）に合わせて、適切なモジュールシステムのJavaScriptに変換することが可能です。

## 📋 まとめとチェックポイント
- モジュールシステムは、コードの可読性、再利用性、保守性を高めるために必須の仕組みである。
- ESM (`import`/`export`) は現在のJavaScriptの標準モジュールシステム。
- CommonJS (`require`/`module.exports`) は主にNode.jsで使われてきた歴史あるモジュールシステム。
- `export default`はモジュールの主役を、`export`は補助的な機能群を公開するのに適している。
- TypeScriptは`tsconfig.json`の設定により、異なるモジュールシステムのJavaScriptにコンパイルできる。

**チェックポイント**:
- ESMとCommonJSの主な違いは何ですか？（ヒント：静的 vs 動的）
- あるモジュールに複数の便利関数を定義しました。エクスポートするには`export default`と`export`のどちらが適していますか？なぜですか？
- `import User from './user'` と `import { User } from './user'` の違いを説明できますか？

## 🔗 関連知識・発展学習
- [0313_TypeScript_Details.md](./0313_TypeScript_Details.md): 型定義（`interface`, `type`）もエクスポートして、他のモジュールで利用することができます。
- **Tree Shaking**: バンドラ（webpackやRollupなど）がESMの静的構造を解析し、実際に使われていない`export`を最終的な成果物から削除する最適化手法。これにより、アプリケーションのファイルサイズを削減できます。 