# 第2章 非同期プログラミング：待ち時間を無駄にしない技術

## 🎯 この章で学ぶこと
- 同期処理と非同期処理の根本的な違いと、なぜUIを持つアプリケーションで非同期処理が不可欠なのかを説明できる。
- かつて主流だったコールバック関数の問題点、特に「コールバック地獄」がなぜコードを読みづらくするのかを理解する。
- `Promise`オブジェクトが「非同期処理の状態（未完了/成功/失敗）」を表現する概念であることを説明できる。
- `async/await`を使い、非同期処理をまるで同期処理のように、直感的に読み書きできるようになる。
- `fetch` APIと`async/await`を組み合わせて、外部APIからデータを取得する、といった実践的な非同期コードを書ける。

## 🤔 なぜ重要なのか
レストランであなたが注文したとします。もしそのレストランが**同期的**にしか動けなかったら、シェフはあなたの料理が完成するまで、他の誰の注文も受け付けられません。あなたがステーキを頼んだら、そのステーキが焼けるまでの15分間、店全体の時間が止まってしまいます。非効率的ですよね？

実際のレストランは**非同期**に動きます。シェフはあなたのステーキをコンロにかけた後、すぐに他の客のサラダの準備に取り掛かります。ステーキが焼けたら、ウェイターがあなたに知らせてくれます。これにより、レストラン全体のスループット（処理能力）は劇的に向上します。

JavaScript、特にブラウザやNode.jsのような環境も、このレストランと全く同じです。
- **重い処理**: 時間のかかるAPI通信、巨大なファイルの読み込みなど。
- **JavaScriptエンジン**: シングルスレッドで動く、一人のシェフ。

もしJavaScriptが同期的にしか動けないと、APIからデータを取得している数秒間、Webページ全体が完全にフリーズしてしまい、ユーザーはボタンをクリックすることも、スクロールすることもできなくなります。これは最悪のユーザー体験です。

**非同期プログラミング**は、この「待ち時間」を有効活用し、アプリケーションの応答性を保つための、JavaScriptの核心的な概念です。`Promise`や`async/await`といった現代的な非同期処理の仕組みを理解することは、快適なUIと高性能なサーバーサイドアプリケーションを構築するための必須要件です。

## 📚 基礎概念の理解

### 同期処理 vs 非同期処理 (Synchronous vs Asynchronous)

- **同期処理**:
    - **振る舞い**: 処理が上から下に順番に実行される。一つの処理が終わるまで、次の処理は開始されない（ブロックする）。
    - **例え**: 電話。相手が応答するまで、自分は他のことができない。
    ```javascript
    console.log('ステップ1');
    // 時間のかかる同期処理（例：alert）
    alert('ここで処理がブロックされます'); 
    console.log('ステップ3'); // alertが閉じられるまで実行されない
    ```

- **非同期処理**:
    - **振る舞い**: ある処理（重い処理）の完了を待たずに、次の処理に進む（ノンブロッキング）。重い処理が終わったら、後で結果を通知してもらう。
    - **例え**: テキストメッセージ。送った後、相手からの返事を待たずに自分の作業を続けられる。
    ```javascript
    console.log('ステップ1');
    // 時間のかかる非同期処理（例：setTimeout）
    setTimeout(() => {
      console.log('ステップ2 (2秒後)'); 
    }, 2000);
    console.log('ステップ3'); // setTimeoutの完了を待たずに即時実行される
    // 実行結果: ステップ1 → ステップ3 → ステップ2
    ```

### Level 1: コールバック関数 (Callback)
非同期処理の最も原始的な形。「この処理が終わったら、この関数（コールバック関数）を実行してね」と、処理完了後のタスクを関数として渡す方法です。

```javascript
function fetchData(callback) {
  setTimeout(() => {
    const data = { name: 'Alice' };
    callback(null, data); // 成功: 第1引数はエラー(null), 第2引数はデータ
  }, 1000);
}

fetchData((error, data) => {
  if (error) {
    console.error('エラー:', error);
  } else {
    console.log('取得データ:', data);
  }
});
```

- **問題点: コールバック地獄 (Callback Hell)**
  非同期処理が連続すると、コールバック関数がどんどんネスト（入れ子）していき、コードが非常に読みづらく、メンテナンス困難になります。この状態は「コールバック地獄」または「破滅のピラミッド」と呼ばれます。
  ```javascript
  step1(data, (result1) => {
    step2(result1, (result2) => {
      step3(result2, (result3) => {
        // ...永遠に続くかのようなインデント
      });
    });
  });
  ```

### Level 2: Promise
コールバック地獄の問題を解決するために導入された、非同期処理をより洗練された形で扱うためのオブジェクトです。

- **`Promise`とは**: 非同期処理の「最終的な結果（成功または失敗）」を表すオブジェクト。作成された時点では結果が不明で、以下の3つの内部状態を持ちます。
    1.  **`pending`**: 未完了。初期状態。
    2.  **`fulfilled`**: 成功。処理が完了し、結果の値を持つ。
    3.  **`rejected`**: 失敗。処理が失敗し、エラー情報を持つ。

`Promise`は一度状態が確定（`fulfilled`または`rejected`）すると、その状態は不変になります。

- **使い方**:
  `Promise`オブジェクトは、`.then()`で成功時の処理、`.catch()`で失敗時の処理をチェーン（連結）して書くことができます。これにより、コールバック地獄のようなネストが解消され、コードがフラットになります。

```javascript
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const data = { name: 'Bob' };
      resolve(data); // 成功を通知
      // reject(new Error('エラー発生！')); // 失敗を通知
    }, 1000);
  });
}

fetchData()
  .then(data => {
    console.log('取得データ:', data);
  })
  .catch(error => {
    console.error('エラー:', error);
  });
```

### Level 3: `async/await`
`Promise`をさらに直感的、つまり**同期処理のように**書けるようにしたシンタックスシュガー（糖衣構文）です。現在、非同期処理を扱う際の最もモダンで主流な方法です。

- **`async`**: 関数宣言の前に付けると、その関数が**必ず`Promise`を返す**非同期関数であることを示す。
- **`await`**: `async`関数の中でのみ使用可能。`Promise`が完了（`fulfilled`または`rejected`）するまで、関数の実行をその場で**一時停止**し、完了したら結果を返す。

```javascript
// 上記のPromise版と全く同じ動作をする
async function displayData() {
  try {
    console.log('データを取得します...');
    const data = await fetchData(); // fetchDataのPromiseが完了するまでここで待つ
    console.log('取得データ:', data);
  } catch (error) {
    console.error('エラー:', error);
  }
}

displayData();
```
`async/await`を使うと、非同期処理のコードが、まるで上から下に順番に実行される同期処理のように読めるため、可読性が劇的に向上します。

## 💡 実践的な活用

### ハンズオン：`fetch`でAPIからデータを取得する
ブラウザに標準で搭載されている`fetch` APIを使って、外部のAPIからデータを取得する処理を`async/await`で書いてみましょう。`fetch`は`Promise`を返す代表的な非同期関数です。

```javascript
// JSONPlaceholderという、ダミーのデータを返してくれるAPIを利用
const API_URL = 'https://jsonplaceholder.typicode.com/users/1';

async function fetchUser() {
  try {
    // 1. fetchでAPIにリクエストを送り、レスポンスを待つ
    const response = await fetch(API_URL);

    // 2. レスポンスが正常でない場合、エラーを投げる
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    // 3. レスポンスのボディをJSONとして解釈する処理を待つ (.json()もPromiseを返す)
    const user = await response.json();

    console.log('ユーザー情報:', user);
    // 例: { id: 1, name: 'Leanne Graham', ... }
    
  } catch (error) {
    console.error('データの取得に失敗しました:', error);
  }
}

fetchUser();
```

### `Promise.all`による並列処理
複数の非同期処理を同時に開始し、その**すべて**が終わるのを待ちたい場合があります。例えば、複数のAPIから同時にデータを取得したいときなどです。`Promise.all`は、このようなケースで役立ちます。

```javascript
async function fetchMultipleData() {
  try {
    const [user, posts] = await Promise.all([
      fetch('https://jsonplaceholder.typicode.com/users/1').then(res => res.json()),
      fetch('https://jsonplaceholder.typicode.com/posts?userId=1').then(res => res.json())
    ]);

    console.log('ユーザー:', user.name);
    console.log('投稿数:', posts.length);
  } catch (error) {
    console.error('いずれかのデータ取得に失敗しました:', error);
  }
}

fetchMultipleData();
```
`Promise.all`に`Promise`の配列を渡すと、それらが並列で実行され、すべての`Promise`が成功した場合にのみ、結果の配列を返します。一つでも失敗すると、即座に`catch`ブロックに移行します。

## 🔍 深掘り：プロの視点

### イベントループの仕組み
JavaScriptがどのようにして「シングルスレッド」なのに非同期処理を実現しているのか、その心臓部が**イベントループ (Event Loop)** です。

```mermaid
graph TD
    subgraph JavaScriptエンジン
        A[コールスタック<br/>(Call Stack)]
    end
    subgraph ブラウザ/Node.js API
        B[Web APIs<br/>(DOM, setTimeout, fetch)]
    end
    subgraph キュー
        C[コールバックキュー<br/>(Callback Queue)]
    end
    
    A -- 'setTimeout(cb, 2000)' --> B;
    B -- 2秒後 -- > C(cbをキューに追加);
    D(イベントループ) -- スタックが空なら --> E{キューからタスクを取り出す};
    E -- 'cb()' --> A;

    linkStyle 3 stroke-width:2px,stroke:blue,stroke-dasharray: 5 5;
```

1.  **コールスタック**: 実行中のコードが積まれる場所。
2.  **Web/Node API**: `setTimeout`や`fetch`のような非同期処理は、JavaScriptエンジン本体ではなく、ブラウザやNode.jsが提供するAPIが担当する。
3.  **コールバックキュー**: 非同期処理が完了した際、そのコールバック関数が待機する行列。
4.  **イベントループ**: コールスタックが空になるたびに、コールバックキューをチェックし、もしタスクがあれば、それを取り出してコールスタックに積んで実行する、という仕事を絶え間なく繰り返す監視者。

この仕組みにより、重い処理をWeb APIに任せている間、JavaScriptのメインスレッド（コールスタック）は他の作業（UIの更新など）を進めることができ、完了通知が来たらイベントループが後始末をしてくれる、という効率的な非同期実行が実現されています。

## 📋 まとめとチェックポイント
- JavaScriptの非同期処理は、アプリケーションの応答性を保ち、待ち時間を有効活用するための必須技術である。
- コールバック地獄の問題は、`Promise`によって解決された。
- `async/await`は、`Promise`ベースの非同期処理を、同期的で読みやすいコードスタイルで書くための現代的な方法である。
- `Promise`は非同期処理の状態（pending, fulfilled, rejected）を持つオブジェクトであり、`async`関数は必ず`Promise`を返す。
- `await`は、`async`関数内で`Promise`の結果が返るまで処理を一時停止する。エラーハンドリングには`try...catch`文を使う。

**セルフチェック**
- [ ] レストランのウェイターの仕事に例えて、同期処理と非同期処理の違いを説明してください。
- [ ] 「コールバック地獄」とはどのようなコードの状態を指しますか？`Promise`や`async/await`は、その問題をどのように解決しますか？
- [ ] `async`と`await`は、それぞれどのような役割を持つキーワードですか？片方だけで使うことはできますか？
- [ ] 2つの異なるAPIからデータを取得し、両方のデータが揃ってから次の処理に進みたいです。どのような方法を使うのが最も効率的ですか？

## 🔗 関連知識・発展学習
- **TypeScript詳細 (`0313_TypeScript_Details.md`)**: `Promise<string>`のように、非同期処理が返す値の型をTypeScriptで明示することで、さらに安全なコードを書くことができます。
- **Node.js基礎 (`0315_Nodejs_Basic.md`)**: Node.jsでは、ファイルI/Oなど、多くの処理が本質的に非同期であり、非同期プログラミングの理解がより一層重要になります。
- **HTTP/HTTPSプロトコル (`0411_HTTP_HTTPS_Protocol.md`)**: `fetch` APIが内部で何を行っているのか、その背景にあるWebの通信プロトコルについて学びます。 