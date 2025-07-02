# 第1章 JavaScript/TypeScriptの基本文法と特徴：Webを動かす言語の今

## 🎯 この章で学ぶこと
- JavaScriptがWebブラウザで唯一ネイティブに動作する言語であり、なぜ「Webの共通言語」と呼ばれるのかを理解する。
- TypeScriptが「JavaScriptのスーパーセット」であるという意味と、それがもたらす「静的型付け」のメリットを説明できる。
- `let`, `const`を使った変数宣言、アロー関数、テンプレートリテラルなど、モダンJavaScriptの基本的な文法を使いこなせる。
- 「動的型付け」（JavaScript）と「静的型付け」（TypeScript）の根本的な違いと、それぞれのトレードオフを説明できる。
- 簡単なTypeScriptコードを書き、それがJavaScriptにコンパイルされるプロセスを理解する。

## 🤔 なぜ重要なのか
Webサイトのボタンをクリックしたらメニューが開く、フォームに入力したらリアルタイムでエラーが表示される...。こうしたWebページ上のあらゆる「動き」を司っているのが**JavaScript**です。現在、PCやスマートフォンのブラウザで直接実行できるプログラミング言語は、実質的にJavaScriptしかありません。そのため、フロントエンド開発（ユーザーの目に触れる部分の開発）を志す上で、JavaScriptの知識は避けて通れません。

しかし、JavaScriptには弱点もありました。その一つが**動的型付け**という性質です。これは、プログラムが実行されるまで、変数にどんな種類のデータ（数値、文字列、オブジェクトなど）が入っているかわからない、という特徴です。小さなプログラムでは柔軟で書きやすいのですが、アプリケーションが大規模で複雑になるにつれて、予期せぬ型のエラー（数値を期待していたのに文字列が入っていた、など）が多発し、バグの温床となっていました。

この問題を解決するために登場したのが**TypeScript**です。TypeScriptは、JavaScriptのすべての機能を含みつつ、その上に「**静的型（Type）**」のシステムを追加した言語です（スーパーセット）。コードを書いている段階で「この変数には数値しか入れてはいけません」といったルールを強制できるため、多くの型関連のエラーを開発段階で撲滅できます。

AIにコード生成を指示する際も、TypeScriptで型を明示的に指定することで、より意図に沿った、堅牢なコードを生成させることが可能です。現代のWeb開発、特にチームでの大規模開発において、TypeScriptは事実上の標準技術となっており、その基礎を理解することはプロへの第一歩です。

## 📚 基礎概念の理解

### JavaScript (JS) とは？
- **定義**: Webページにインタラクティブな機能を追加するための、高水準なプログラミング言語。
- **実行環境**: 主にWebブラウザ。近年ではNode.jsによってサーバーサイドでも動作する。
- **特徴**:
    - **動的型付け**: 変数の型を実行時に決定する。柔軟性が高いが、予期せぬエラーも起きやすい。
    - **プロトタイプベース**: クラスベースのオブジェクト指向とは少し異なる、独自のオブジェクト継承モデルを持つ。
    - **シングルスレッド/イベントループ**: 非同期処理を効率的に扱うための仕組みを持つ（詳細は別章）。

### TypeScript (TS) とは？
- **定義**: Microsoftによって開発された、JavaScriptに**静的な型システム**と**クラスベースのオブジェクト指向**の概念を追加した、JavaScriptの厳密なスーパーセット。
- **関係性**: すべての有効なJavaScriptコードは、有効なTypeScriptコードでもある。TypeScriptのコードは、最終的に**トランスパイラ**（Babelやtsc）によってプレーンなJavaScriptコードに変換（コンパイル）されてから実行される。

```mermaid
graph LR
    A[TypeScriptコード (.ts)] -- tscなどのコンパイラ --> B(JavaScriptコード (.js));
    B -- 実行 --> C{実行環境 (ブラウザ, Node.js)};
```
- **主なメリット**:
    - **静的型チェック**: コードを書いている段階（コンパイル時）に型の不一致を検知できるため、多くのバグを未然に防げる。
    - **優れたエディタサポート**: 型情報があるため、コード補完（インテリセンス）、リファクタリング、エラー検出といったIDEの機能が非常に強力になる。
    - **可読性と保守性の向上**: 型定義がドキュメントの役割を果たし、大規模なコードでも意図が理解しやすくなる。

### 動的型付け vs 静的型付け

| 特徴       | 動的型付け (JavaScript)                                | 静的型付け (TypeScript)                                    |
| :--------- | :----------------------------------------------------- | :--------------------------------------------------------- |
| **型チェック** | 実行時 (Runtime)                                       | コンパイル時 (Compile-time)                                |
| **コード例**   | `let x = 10;`<br/>`x = "hello"; // エラーにならない` | `let x: number = 10;`<br/>`x = "hello"; // 🚨コンパイルエラー` |
| **長所**     | - 書き始めるのが速い<br/>- コードが短い傾向がある       | - 多くのバグを早期に発見<br/>- IDEの支援が強力<br/>- 大規模開発向き |
| **短所**     | - 実行するまで型エラーが不明<br/>- IDEの支援が限定的   | - 型定義の記述が必要<br/>- コンパイルの手間がかかる         |

### モダンJavaScriptの基本文法
ES2015 (ES6) 以降に導入された、現代的なJavaScript/TypeScriptで頻繁に使われる文法です。

- **変数宣言: `let` と `const`**
    - `var`: 再宣言可能、スコープが広いなど、意図せぬ挙動の原因になりやすいため、現在では非推奨。
    - `let`: 再代入が可能な変数を宣言する。ブロックスコープ（`{}`の中だけで有効）。
    - `const`: 再代入が不可能な定数（値が固定された変数）を宣言する。ブロックスコープ。原則として、変数はまず`const`で定義し、再代入が必要な場合のみ`let`を使うのが良い習慣です。

- **アロー関数 (Arrow Functions)**
    - 従来の`function`キーワードより短く、簡潔に関数を書くための記法。
    - `this`の束縛のされ方が異なるという重要な違いもある（詳細は発展学習で）。
    ```javascript
    // 従来の関数
    function add(a, b) {
      return a + b;
    }
    // アロー関数
    const addArrow = (a, b) => a + b;
    ```

- **テンプレートリテラル (Template Literals)**
    - バッククォート (`` ` ``) を使って文字列を定義する。
    - 文字列内に`${...}`という形式で変数を簡単に埋め込める。
    - 複数行の文字列を簡単に書ける。
    ```javascript
    const name = "World";
    const greeting = `Hello, ${name}!
    This is a multi-line string.`;
    ```

## 💡 実践的な活用

### ハンズオン：JSのDOM操作をTSで安全にする
ボタンをクリックしたら、メッセージを表示する簡単な例で、JavaScriptとTypeScriptの違いを体験します。

**1. JavaScript版**
**`index.html`**
```html
<!DOCTYPE html>
<html>
<body>
  <button id="myButton">Click me</button>
  <p id="message"></p>
  <script src="app.js"></script>
</body>
</html>
```
**`app.js`**
```javascript
const button = document.getElementById('myButton');
const message = document.getElementById('message');

button.addEventListener('click', () => {
  // もしmessage要素が存在しなくても、実行時までエラーは出ない
  message.textContent = 'Hello, JavaScript!';
});
```
もしHTMLの`p`タグのidを`message`から`msg`に変えてしまうと、`message`は`null`になり、クリック時に`TypeError: Cannot set properties of null`という実行時エラーが発生します。

**2. TypeScript版**
**準備**:
- `npm init -y`
- `npm install --save-dev typescript`
- `npx tsc --init` (tsconfig.jsonを作成)

**`app.ts`**
```typescript
// 型を指定することで、要素が存在しない可能性をエディタが警告してくれる
const button = document.getElementById('myButton') as HTMLButtonElement;
const message = document.getElementById('message') as HTMLParagraphElement;

// buttonがnullの可能性があるため、オプショナルチェイニング(?.)を使うとより安全
button?.addEventListener('click', () => {
  if (message) {
    message.textContent = 'Hello, TypeScript!';
  }
});
```
TypeScriptでは、`getElementById`が返す値は`HTMLElement | null`（HTMLElementまたはnull）という型になります。これにより、`message`が`null`である可能性をコードを書いている時点で意識させられます。`if (message)`のようなnullチェックを強制されることで、実行時エラーを未然に防ぐことができます。

**コンパイル**:
ターミナルで`npx tsc`を実行すると、`app.ts`がコンパイルされて`app.js`が生成されます。HTMLファイルからは、この生成された`app.js`を読み込みます。

## 🔍 深掘り：プロの視点

### `any` vs `unknown`
TypeScriptでは、型が不明な場合に使える特別な型があります。
- `any`: **最終手段**。どんな型としても振る舞え、どんな操作も許してしまう「魔法の型」。静的型チェックを完全に無効にするため、多用するとTypeScriptのメリットが失われる。既存のJavaScriptコードを移行する際の、一時的な利用に留めるべき。
- `unknown`: **安全な`any`**。`any`と同様にどんな型の値も代入できるが、利用する際には「型ガード」や「型アサーション」によって、型を明確にしないと操作ができない。型が不明な外部からの入力（APIレスポンスなど）を受け取る際に有用。

```typescript
let value: unknown;
value.toFixed(2); // 🚨 エラー: 'value' is of type 'unknown'.

if (typeof value === 'number') {
  value.toFixed(2); // OK! 型ガードでnumberだと確定させた
}
```

### 型推論 (Type Inference)
TypeScriptは、開発者がすべての型を明記しなくても、文脈から型を自動的に推測してくれる**型推論**の機能を持っています。
```typescript
let name = "Alice"; // TypeScriptはnameをstring型だと推論する
name = 123; // 🚨 エラー: Type 'number' is not assignable to type 'string'.
```
この機能のおかげで、コードの冗長性を抑えつつ、静的型付けの恩恵を受けることができます。

## 📋 まとめとチェックポイント
- JavaScriptはWebの共通言語であり、動的型付けという柔軟だがエラーを起こしやすい性質を持つ。
- TypeScriptは、JavaScriptに静的型付けを追加したスーパーセットで、コードの安全性と保守性を劇的に向上させる。
- TypeScriptで書かれたコードは、最終的にJavaScriptにコンパイルされて実行される。
- `let`/`const`、アロー関数、テンプレートリテラルは、モダンなJS/TS開発の基本文法である。
- 静的型付けは、バグを実行時ではなく開発時に発見することを可能にし、特に大規模開発でその真価を発揮する。

**セルフチェック**
- [ ] あなたの同僚が「TypeScriptは覚えることが多くて面倒だ。JavaScriptで十分だ」と主張しています。TypeScriptを導入するメリットを3つ挙げて、彼を説得してみてください。
- [ ] `let`, `const`, `var` の違い、特に `let` と `const` をどう使い分けるべきかを説明できますか？
- [ ] APIから受け取った、中身が不明なデータを格納する変数があります。この変数の型は `any` と `unknown` のどちらにすべきですか？その理由も説明してください。
- [ ] TypeScriptの「型推論」とは何か、簡単なコード例を挙げて説明してください。

## 🔗 関連知識・発展学習
- **非同期プログラミング (`0312_Asynchronous_Programming.md`)**: JavaScript/TypeScriptの核心的な特徴の一つである非同期処理（Promise, async/await）について深く学びます。
- **DOM操作 (`0432_DOM_Manipulation.md`)**: JavaScriptがWebページをどのように操作するのか、その仕組みを学びます。
- **モジュールシステム (`0314_Module_System.md`)**: コードをファイル単位で分割し、再利用するための仕組み（ES Modules, CommonJS）について学びます。 