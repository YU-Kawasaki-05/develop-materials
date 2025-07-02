# 3.2 SQL基礎から応用

## 🎯 この章で学ぶこと
- SQL（Structured Query Language）の役割と基本的な構文を理解する。
- データの取得（SELECT）、追加（INSERT）、更新（UPDATE）、削除（DELETE）を自由に行えるようになる。
- 複数のテーブルを結合（JOIN）して、複雑な条件でデータを取得する方法を習得する。
- `GROUP BY`や集約関数を使い、データの集計・分析ができるようになる。
- SQLインジェクションなどのセキュリティリスクを理解し、安全なコードを書くための基礎知識を身につける。

## 🤔 なぜ重要なのか
前の章でリレーショナルデータベースの「構造」を学びましたが、その構造化されたデータを実際に操作するための言語が**SQL**です。プログラミング言語がアプリケーションのロジックを記述するのに対し、SQLはデータベースとの対話に特化しています。

AIは基本的なSQLクエリを生成できますが、複雑なデータ要件を満たすための最適なクエリを書くことは依然として人間の専門知識を必要とします。特に、パフォーマンスを考慮したクエリのチューニングや、複数のテーブルを効率的に結合するロジックは、ビジネスの要件を深く理解しているエンジニアの腕の見せ所です。SQLを理解することで、データの流れを正確にコントロールし、アプリケーションのパフォーマンスを最大化することができます。

## 📚 基礎概念の理解

### SQLとは何か
SQLは、リレーショナルデータベースを操作するための国際標準言語です。その役割は大きく3つに分類されます。

- **DML (Data Manipulation Language): データ操作言語**
    - テーブル内のデータを操作するための命令群。
    - 例: `SELECT` (検索), `INSERT` (追加), `UPDATE` (更新), `DELETE` (削除)
- **DDL (Data Definition Language): データ定義言語**
    - データベースの構造（テーブルやインデックスなど）を定義するための命令群。
    - 例: `CREATE TABLE` (テーブル作成), `ALTER TABLE` (テーブル変更), `DROP TABLE` (テーブル削除)
- **DCL (Data Control Language): データ制御言語**
    - データベースへのアクセス権限を管理するための命令群。
    - 例: `GRANT` (権限付与), `REVOKE` (権限剥奪)

初心者のうちは、特に**DML**と**DDL**を重点的に学習します。

### SELECT文の基本（射影・選択）
`SELECT`文は、テーブルからデータを取得するための最も基本的で重要な命令です。

- **射影 (Projection)**: テーブルから特定の**カラム（列）**を選んで表示すること。
    - `SELECT title, content FROM posts;` -- `posts`テーブルから`title`と`content`カラムのみ取得
- **選択 (Selection)**: テーブルから特定の条件に合う**レコード（行）**を選んで表示すること。
    - `SELECT * FROM posts WHERE user_id = 1;` -- `posts`テーブルから`user_id`が1のレコードをすべて取得（`*`は全カラムを意味する）

### データ操作 (INSERT, UPDATE, DELETE)
- **INSERT**: 新しいレコードをテーブルに追加します。
    - `INSERT INTO users (username, email) VALUES ('田中 宏', 'tanaka@example.com');`
- **UPDATE**: 既存のレコードの情報を更新します。
    - `UPDATE users SET email = 'new.tanaka@example.com' WHERE username = '田中 宏';`
    - **注意**: `WHERE`句を忘れると、テーブルの全レコードが更新されてしまうため、絶対に忘れないでください！
- **DELETE**: 既存のレコードを削除します。
    - `DELETE FROM users WHERE username = '田中 宏';`
    - **注意**: `UPDATE`と同様に、`WHERE`句がないと全レコードが削除されます。

### データ定義 (CREATE TABLE, ALTER TABLE)
- **CREATE TABLE**: 新しいテーブルを作成します。
    - カラム名とその**データ型**（`INT`, `VARCHAR`, `TEXT`, `TIMESTAMP`など）を指定します。
    ```sql
    CREATE TABLE products (
        product_id INT PRIMARY KEY,
        product_name VARCHAR(255) NOT NULL,
        price DECIMAL(10, 2),
        created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    );
    ```
- **ALTER TABLE**: 既存のテーブルの構造を変更します。
    - `ALTER TABLE products ADD COLUMN description TEXT;` -- `description`カラムを追加

## 💡 実践的な活用

### よく使う句
`SELECT`文と組み合わせて、より複雑なデータ抽出を行います。

- **`WHERE`**: 抽出するレコードの条件を指定する。
- **`ORDER BY`**: 結果を特定のカラムの値で並べ替える (`ASC`: 昇順, `DESC`: 降順)。
    - `SELECT * FROM posts ORDER BY created_at DESC;` -- 新しい記事から順に表示
- **`GROUP BY`**: 特定のカラムの値でレコードをグループ化し、集約関数を適用する。
- **`HAVING`**: `GROUP BY`でグループ化された結果に対して、さらに条件を指定する。

**集約関数 (Aggregate Functions)**
- `COUNT()`: レコード数を数える
- `SUM()`: 数値の合計を計算する
- `AVG()`: 平均値を計算する
- `MAX()`/`MIN()`: 最大値/最小値を求める
    - `SELECT user_id, COUNT(*) FROM posts GROUP BY user_id;` -- ユーザーごとの記事投稿数を計算

### テーブルの結合 (JOIN)
複数のテーブルを関連付けて、一度にデータを取得する強力な機能です。

- **INNER JOIN**: 両方のテーブルに存在するレコードのみを結合する。
    - `users`テーブルと`posts`テーブルを結合して、記事の投稿者名を取得する。
    ```sql
    SELECT p.title, u.username
    FROM posts AS p
    INNER JOIN users AS u ON p.user_id = u.user_id;
    ```
- **LEFT JOIN (or LEFT OUTER JOIN)**: 左側のテーブルのレコードはすべて残し、右側のテーブルに一致するレコードがあれば結合する（なければ`NULL`になる）。
    - 全ユーザーの一覧と、もし記事を投稿していればそのタイトルを取得する（投稿がなくてもユーザーは表示される）。
    ```sql
    SELECT u.username, p.title
    FROM users AS u
    LEFT JOIN posts AS p ON u.user_id = p.user_id;
    ```

### ハンズオン：ブログ記事のデータをSQLで取得する
前章で設計したブログシステムのテーブルを使って、以下の要求を満たすSQLを書いてみましょう。

- **課題**: 「山田 太郎」さんが投稿した記事のうち、最新の記事に付いたコメントをすべて取得してください。
- **期待される結果**:
    - 「山田 太郎」さんの最新記事に付いたコメントのテキストと、そのコメント投稿者のユーザー名が表示される。

- **SQLクエリの組み立て（思考プロセス）**:
    1.  `users`テーブルから「山田 太郎」さんの`user_id`を見つける。
    2.  その`user_id`を使って`posts`テーブルから記事を検索し、`ORDER BY created_at DESC LIMIT 1`で最新の記事の`post_id`を特定する。
    3.  その`post_id`を使って`comments`テーブルからコメントをすべて検索する。
    4.  取得したコメントの`user_id`を使い、`users`テーブルを再度結合してコメント投稿者の名前を取得する。

    ```sql
    -- 答えの一例（サブクエリを使用）
    SELECT c.comment_text, u.username AS commenter_name
    FROM comments AS c
    JOIN users AS u ON c.user_id = u.user_id
    WHERE c.post_id = (
        SELECT post_id
        FROM posts
        WHERE user_id = (SELECT user_id FROM users WHERE username = '山田 太郎')
        ORDER BY created_at DESC
        LIMIT 1
    );
    ```

## 🔍 深掘り：プロの視点

### サブクエリと相関サブクエリ
- **サブクエリ (Subquery)**: `WHERE`句や`FROM`句の中に書かれる`SELECT`文です。上記のハンズオンの例でも使いました。外側のクエリが実行される前に一度だけ実行されます。
- **相関サブクエリ (Correlated Subquery)**: 内側のクエリが外側のクエリのカラムを参照するサブクエリです。外側のクエリの各行に対して一度ずつ実行されるため、パフォーマンスが低下しやすい傾向にあり、`JOIN`で書き換えられないか検討することが重要です。

### ウィンドウ関数 (Window Functions)
`GROUP BY`が集約して行数を減らすのに対し、ウィンドウ関数は元の行数を保ったまま、各行に対して集約的な計算（順位、累計、移動平均など）を行える強力な機能です。
- `RANK()`: 順位を計算する（同じ値は同じ順位になり、次の順位は飛ぶ）
- `SUM(...) OVER (...)`: パーティション（区切り）ごとの累計を計算する
- **例**: カテゴリごとに、価格の高い順に商品のランキングを付ける。
    ```sql
    SELECT
        product_name,
        category_name,
        price,
        RANK() OVER (PARTITION BY category_name ORDER BY price DESC) AS rank_in_category
    FROM products;
    ```

### SQLインジェクション (SQL Injection) と対策
- **概要**: 悪意のあるユーザーが、アプリケーションの入力フォームなどを通じて不正なSQL文を注入（inject）し、データベースを不正に操作する攻撃です。
- **攻撃例**: ユーザーID入力欄に `1 OR 1=1` と入力されると、`WHERE user_id = 1 OR 1=1` という常に真になる条件が作られ、全ユーザーの情報が漏洩する可能性がある。
- **対策**:
    - **プリペアドステートメント (Prepared Statements)**: SQL文の「型枠」を先にデータベースに送信し、後からパラメータを「値」として渡す方法。SQL文の構造が固定されるため、注入を防げます。これは**絶対に遵守すべき**最も重要な対策です。

## 📋 まとめとチェックポイント
- SQLはデータベースと対話するための言語で、DML、DDL、DCLに大別される。
- `SELECT`, `INSERT`, `UPDATE`, `DELETE`がデータ操作の基本。特に`UPDATE`と`DELETE`での`WHERE`句のつけ忘れには細心の注意を払う。
- `JOIN`を使うことで、正規化された複数のテーブルから効率的にデータを取得できる。
- `GROUP BY`と集約関数はデータ分析の基本。さらに高度な分析にはウィンドウ関数が有用。
- セキュリティ対策として、SQLインジェクションを防ぐためのプリペアドステートメントの利用は必須。

**セルフチェック**
- [ ] DMLとDDLの違いを具体例を挙げて説明できますか？
- [ ] `INNER JOIN`と`LEFT JOIN`の違いと、それぞれの使い分けのシナリオを説明できますか？
- [ ] `WHERE`と`HAVING`の違いを説明できますか？
- [ ] なぜSQLインジェクションが危険なのか、その基本的な対策方法と共に説明できますか？

## 🔗 関連知識・発展学習
- **3.1 リレーショナルデータベース**: この章で操作しているデータベースの構造に関する知識を再確認しましょう。
- **17.3 セキュアコーディング**: SQLインジェクションを含む、より広範なセキュリティの脅威と対策について学びます。
- **13.2 データベース連携**: 実際のプログラミング言語（PythonやJavaScriptなど）から、どのようにして安全にSQLを実行するのか（プリペアドステートメントの実装方法など）を学びます。 