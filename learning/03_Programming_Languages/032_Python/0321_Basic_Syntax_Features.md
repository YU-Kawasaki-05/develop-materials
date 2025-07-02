# 0321 Pythonの基本文法と特徴

## 🎯 この章で学ぶこと
- Pythonが「読みやすく、書きやすい」と言われる理由である、シンプルな文法構造を理解する。
- インデントがコードのブロックを定義するという、Pythonの最も特徴的なルールを習得する。
- 数値、文字列、リスト、タプル、辞書、集合といった、Pythonの主要な組み込みデータ型を使いこなせるようになる。
- Pythonにおける動的型付けの性質と、それがもたらす柔軟性および注意点を理解する。
- `if`, `for`, `while`を使った基本的な制御フローと、関数の定義・呼び出し方法を学ぶ。

## 🤔 なぜ重要なのか
Pythonは、Web開発、データサイエンス、機械学習、自動化スクリプトなど、非常に幅広い分野で採用されている、現代で最も人気のあるプログラミング言語の一つです。その最大の理由は「**学習のしやすさ**」と「**汎用性の高さ**」にあります。

文法が人間にとって自然言語に近く、コードの記述量が少ないため、プログラミング初心者が最初に学ぶ言語として最適です。また、豊富なライブラリ（特にデータ分析やAI関連）が揃っているため、初心者から専門家まで、あらゆるレベルの開発者が強力なツールとして活用できます。

AIに開発を依頼する際も、Pythonの基本的な文法や思想を理解していることは極めて重要です。これにより、生成されたコードの品質を判断したり、意図通りに修正したり、あるいは「Pythonらしく（Pythonicな）コードを書いて」といった、より高度な指示を出すことが可能になります。

## 📚 基礎概念の理解

### インデントによるブロック表現
多くの言語が `{}` (波括弧) でコードのまとまり（ブロック）を表現するのに対し、Pythonは**インデント（字下げ）**でブロックを表現します。これはPythonの最も特徴的なルールです。

```python
# 正しいインデントの例
def say_hello(name):
    if name:
        print(f"Hello, {name}!") # ifブロックの中
    else:
        print("Hello, World!")   # elseブロックの中
    print("Function end.")     # 関数のブロックの中

# 間違ったインデントの例
def say_hello_bad(name):
print("This causes an IndentationError") # インデントがないためエラー
```
このルールにより、誰が書いてもコードの見た目が統一され、高い可読性が保たれます。

### 動的型付け
Pythonは**動的型付け言語**です。これは、変数の型を事前に宣言する必要がなく、代入される値によって実行時に型が自動的に決まることを意味します。

```python
message = "こんにちは" # この時点でmessageは文字列型(str)
print(type(message)) # <class 'str'>

message = 123        # 再代入すると数値型(int)に変わる
print(type(message)) # <class 'int'>
```
これにより、柔軟で簡潔なコードを書けますが、意図しない型が代入されてエラーを引き起こす可能性もあるため、注意が必要です。（最近では、この点を補うための「型ヒント」という仕組みも導入されています。）

### 主要なデータ型
Pythonには、強力な組み込みデータ型が用意されています。

- **数値型**: `int` (整数), `float` (浮動小数点数)
- **文字列型 (`str`)**: `' '` または `"` `" `で囲んだテキスト。
- **リスト (`list`)**: 順序があり、変更可能な要素のコレクション。`[1, "apple", 3.14]`
- **タプル (`tuple`)**: 順序があるが、**変更不可能**な要素のコレクション。`(1, "apple", 3.14)`
- **辞書 (`dict`)**: キーと値のペアを格納するコレクション。`{'name': 'Alice', 'age': 30}`
- **集合 (`set`)**: 順序がなく、**重複する要素を持たない**コレクション。`{1, 2, 3}`

これらのデータ型を適切に使い分けることが、効率的でPythonicなコードを書く第一歩です。

## 💡 実践的な活用

### ハンズオン：簡単な単語カウンター
テキストファイルに含まれる各単語の出現回数を数えるプログラムを作成してみましょう。

1.  **準備**: `sample.txt`という名前で、簡単な英文を含むファイルを作成します。
    ```
    hello world this is a test
    python is fun and python is easy
    ```
2.  **コード (`word_counter.py`)**:
    ```python
    def count_words(filepath):
        """指定されたファイルの単語出現回数を数える"""
        word_counts = {} # 結果を格納する辞書を初期化

        try:
            with open(filepath, 'r', encoding='utf-8') as f:
                for line in f:
                    words = line.lower().split() # 小文字に変換して単語に分割
                    for word in words:
                        # 辞書のgetメソッドで、キーが存在すればその値を、なければ0を返す
                        word_counts[word] = word_counts.get(word, 0) + 1
        except FileNotFoundError:
            print(f"エラー: ファイル '{filepath}' が見つかりません。")
            return None

        return word_counts

    # 実行部分
    counts = count_words('sample.txt')
    if counts:
        # sorted関数とlambda式で、回数が多い順にソートして表示
        sorted_counts = sorted(counts.items(), key=lambda item: item[1], reverse=True)
        for word, count in sorted_counts:
            print(f"'{word}': {count}回")
    ```
**期待される結果**:
```
'is': 2回
'python': 2回
'a': 1回
'world': 1回
'this': 1回
'test': 1回
'hello': 1回
'easy': 1回
'and': 1回
'fun': 1回
```
この短いコードの中に、ファイル操作(`with open`)、辞書(`dict`)、ループ(`for`)、関数の定義(`def`)、エラー処理(`try-except`)など、Pythonの基本要素が詰まっています。

## 🔍 深掘り：プロの視点

### Pythonic（パイソニック）なコード
Pythonコミュニティには、「Pythonらしい」書き方、すなわち**Pythonic**なコードという考え方があります。これは単に文法的に正しいだけでなく、Pythonの設計思想に沿った、簡潔で読みやすいコードを指します。

- **例：ループの書き方**
  ```python
  numbers = [10, 20, 30]

  # C言語風（Pythonicではない）
  for i in range(len(numbers)):
      print(numbers[i])

  # Pythonicな書き方
  for num in numbers:
      print(num)
  ```
  インデックスを直接操作するのではなく、イテラブルなオブジェクト（リストなど）の要素を直接ループで回すのがPythonicです。

- **内包表記 (Comprehensions)**
  リスト、辞書、集合を非常に簡潔に作成するための構文です。
  ```python
  # 0から9の2乗を要素に持つリストを作成
  # 伝統的な方法
  squares = []
  for i in range(10):
      squares.append(i * i)

  # リスト内包表記 (Pythonic)
  squares = [i * i for i in range(10)]
  ```
内包表記を使いこなすことは、中級者への第一歩と言えます。

### スクリプト言語としての強み
Pythonはコンパイル不要のインタプリタ言語であり、書いたコードをすぐに実行できます。この手軽さから、日々の定型作業を自動化する「スクリプト」の作成に非常に向いています。
- ファイルの整理
- Webサイトからのデータ収集（スクレイピング）
- ExcelやCSVファイルの処理
- 簡単なAPIの呼び出し

「ちょっとした面倒な作業」をPythonスクリプトで自動化するスキルは、あらゆる職種の開発者にとって強力な武器となります。

## 📋 まとめとチェックポイント
- Pythonはインデントでコードブロックを管理する、シンプルで可読性の高い言語である。
- 変数の型を宣言しない動的型付け言語であり、柔軟性に富む。
- `list`, `tuple`, `dict`, `set` などの強力な組み込みデータ型が特徴。
- Pythonicなコードとは、Pythonの思想に沿った簡潔で読みやすいコードのこと。
- 対話的な実行やスクリプト作成が容易で、幅広い用途に活用できる。

**チェックポイント**:
- Pythonの`list`と`tuple`の最も重要な違いは何ですか？どのような場合に使い分けますか？
- Pythonのインデントルールを他の言語の`{}`と比較して、メリットとデメリットを説明できますか？
- `squares = [i * i for i in range(10)]` というコード（リスト内包表記）が何をしているか説明できますか？

## 🔗 関連知識・発展学習
- [0122_Object_Oriented_Programming.md](../../01_Development_Basic/012_Programming_Concepts/0122_Object_Oriented_Programming.md): Pythonは強力なオブジェクト指向言語でもあります。`class`構文について学ぶと、より大規模なアプリケーションを構築できます。
- [0322_Library_Package_Management.md](./0322_Library_Package_Management.md): Pythonの真の力は、`pip`を使ってインストールできる豊富な外部ライブラリにあります。
- **公式ドキュメント**: [Pythonチュートリアル](https://docs.python.org/ja/3/tutorial/index.html) 