# 0323 データ処理とスクリプト

## 🎯 この章で学ぶこと
- Pythonがデータサイエンスや分析の分野で絶大な人気を誇る理由を理解する。
- データ分析に必須のライブラリである`Pandas`の基本操作（`DataFrame`の作成、読み書き、簡単な集計）を習得する。
- グラフ描画ライブラリである`Matplotlib`を使い、分析結果を可視化する基本的な方法を学ぶ。
- Webスクレイピングの概念と、`Requests`と`Beautiful Soup`を使った簡単なデータ収集方法を理解する。
- 日常の定型業務を自動化する「自動化スクリプト」の考え方と、その具体例に触れる。

## 🤔 なぜ重要なのか
現代のビジネスや研究において、データに基づいた意思決定は不可欠です。大量のデータを効率的に収集し、整形し、分析し、そして可視化する能力は、多くの開発者にとって重要なスキルセットとなっています。Pythonはこの一連のプロセスを極めて効率的に行うためのツールが揃っており、「データ処理といえばPython」と言われるほどの地位を確立しています。

また、私たちの周りには「毎月このExcelファイルを手作業で集計している」「定期的にWebサイトをチェックして情報をコピペしている」といった、単純な繰り返し作業（定型業務）が溢れています。Pythonを使えば、このような退屈な作業を自動化するスクリプトを簡単に作成できます。

この章で学ぶスキルは、AI開発の文脈においても直接役立ちます。AIモデルに学習させるためのデータを準備（前処理）する際や、モデルの出力結果を分析・可視化する際に、ここで登場するライブラリがフル活用されるからです。

## 📚 基礎概念の理解

### データ処理の主役: Pandas
`Pandas`は、Pythonで構造化データ（表形式のデータなど）を扱うための、最も重要で強力なライブラリです。Excelのスプレッドシートを、より高機能かつプログラム可能にしたものと考えると分かりやすいです。

- **`Series`**: 1次元のラベル付き配列。Excelの1列に相当します。
- **`DataFrame`**: 2次元のラベル付きデータ構造。行と列を持つ、Excelのシート全体に相当する、Pandasで最も中心的なオブジェクトです。

`DataFrame`を使うことで、CSVファイルやExcelファイルを数行のコードで読み込み、データのフィルタリング、並べ替え、欠損値の処理、集計などを自由自在に行うことができます。

### 可視化の基本: Matplotlib
`Matplotlib`は、Pythonでグラフを描画するための定番ライブラリです。`Pandas`と連携させることで、`DataFrame`のデータを元に折れ線グラフ、棒グラフ、散布図などを簡単に作成できます。データの傾向やパターンを視覚的に捉えることは、分析において極めて重要です。

### Webスクレイピングの道具
Web上に公開されている情報をプログラムで自動的に収集する技術を**Webスクレイピング**と呼びます。
- **`Requests`**: HTTP通信を簡単に行うためのライブラリ。指定したURLのHTMLコンテンツを取得するのに使います。
- **`Beautiful Soup`**: `Requests`が取得してきたHTMLの文字列を解析し、特定の要素（見出し、リンク、表など）を簡単に抽出できるようにしてくれるライブラリ。

この2つを組み合わせることで、Web上の情報を効率的に収集し、データとして活用することができます。

## 💡 実践的な活用

### ハンズオン：簡単なCSVデータ分析と可視化
架空の売上データ（`sales.csv`）を分析し、グラフ化してみましょう。

1.  **準備**:
    -   仮想環境で`pandas`と`matplotlib`をインストールします。
        ```bash
        pip install pandas matplotlib
        ```
    -   以下の内容で`sales.csv`ファイルを作成します。
        ```csv
        Date,Category,Revenue
        2023-01-01,Fruit,150
        2023-01-01,Vegetable,80
        2023-01-02,Fruit,180
        2023-01-02,Bakery,120
        2023-01-03,Vegetable,90
        2023-01-03,Fruit,210
        ```

2.  **分析と可視化コード (`analyze.py`)**:
    ```python
    import pandas as pd
    import matplotlib.pyplot as plt

    # CSVファイルをDataFrameとして読み込む
    try:
        df = pd.read_csv('sales.csv')
    except FileNotFoundError:
        print("sales.csvが見つかりません。")
        exit()

    # --- 基本的なデータ分析 ---
    print("--- データの最初の5行 ---")
    print(df.head())

    print("\n--- 基本統計量 ---")
    print(df.describe())

    # カテゴリ別の合計売上を計算
    category_sales = df.groupby('Category')['Revenue'].sum()
    print("\n--- カテゴリ別合計売上 ---")
    print(category_sales)


    # --- データの可視化 ---
    # カテゴリ別売上を棒グラフで表示
    category_sales.plot(kind='bar', title='Total Revenue by Category')
    plt.ylabel('Total Revenue')
    plt.xticks(rotation=0) # X軸のラベルを水平に
    plt.tight_layout() # レイアウトを調整
    plt.savefig('category_sales.png') # グラフを画像として保存
    print("\nカテゴリ別売上グラフを 'category_sales.png' として保存しました。")
    ```

**期待される結果**:
コンソールに分析結果が表示され、同じディレクトリに`category_sales.png`という棒グラフの画像ファイルが生成されます。この一連の流れ（データ読み込み→集計→可視化）は、データ分析の基本的なワークフローです。

### 自動化スクリプトの例：ファイルの整理
デスクトップに散らばったファイルを、拡張子ごとにフォルダへ自動で振り分けるスクリプトを考えてみましょう。

```python
import os
import shutil

# 整理したいディレクトリ（例：デスクトップ）
# 実際のパスに書き換えてください
TARGET_DIR = os.path.expanduser("~/Desktop") 

def organize_files(directory):
    for filename in os.listdir(directory):
        # ファイルでないもの（ディレクトリなど）はスキップ
        if not os.path.isfile(os.path.join(directory, filename)):
            continue

        # 拡張子を取得
        file_ext = os.path.splitext(filename)[1][1:] # '.txt' -> 'txt'
        if not file_ext: # 拡張子がないファイルはスキップ
            continue

        # 拡張子と同じ名前のディレクトリを作成
        dest_dir = os.path.join(directory, file_ext)
        os.makedirs(dest_dir, exist_ok=True) # 既に存在していてもエラーにしない

        # ファイルを移動
        shutil.move(os.path.join(directory, filename), dest_dir)
        print(f"Moved '{filename}' to '{dest_dir}'")

if __name__ == '__main__':
    organize_files(TARGET_DIR)
```
このスクリプトを実行するだけで、面倒なファイル整理が一瞬で完了します。このように、OSの操作（`os`モジュール）やファイルの操作（`shutil`モジュール）と組み合わせることで、PC上のあらゆる定型作業を自動化の対象にできます。

## 🔍 深掘り：プロの視点

### データ分析ライブラリの生態系
`Pandas`と`Matplotlib`は基本ですが、Pythonのデータサイエンスエコシステムはさらに豊かです。
- **NumPy**: 数値計算、特に多次元配列（行列）の操作を高速に行うためのライブラリ。`Pandas`の内部でも利用されており、科学技術計算の基盤です。
- **SciPy**: `NumPy`をベースにした、より高度な科学技術計算（信号処理、統計、最適化など）のためのライブラリ。
- **Seaborn**: `Matplotlib`をベースに、より美しく、統計的なグラフを簡単に描けるようにしたライブラリ。
- **Scikit-learn**: 機械学習のための定番ライブラリ。分類、回帰、クラスタリングなど、主要なアルゴリズムを統一されたインターフェースで提供します。

これらのライブラリ群がシームレスに連携することで、Pythonはデータ収集から前処理、分析、可視化、そして機械学習モデルの構築まで、一気通貫で行える強力なプラットフォームとなっています。

### スクリプト実行の高速化と並列処理
ほとんどのスクリプトは逐次実行で十分ですが、大量のデータを扱ったり、多数のWebサイトをスクレイピングしたりする場合、実行速度が問題になることがあります。
- **`multiprocessing`**: Pythonの標準ライブラリで、処理を複数のCPUコアに割り当てて並列実行することができます。CPUに負荷のかかる計算を高速化するのに有効です。
- **`asyncio`**: ネットワーク通信のように、待機時間（I/Oバウンド）が多い処理を効率化するための非同期処理フレームワーク。多数のAPIを同時に叩いたり、多数のWebページを同時にダウンロードしたりする際に絶大な効果を発揮します。

適切なツールを選択することで、単純なスクリプトを、より高性能なデータ処理パイプラインへとスケールアップさせることが可能です。

## 📋 まとめとチェックポイント
- `Pandas`はPythonでの表形式データ処理の中心的なライブラリであり、`DataFrame`がその主役。
- `Matplotlib`はデータ分析結果をグラフとして可視化するための基本的なツール。
- `Requests`と`Beautiful Soup`を組み合わせることで、Webスクレイピングによるデータ収集が可能。
- Pythonの標準ライブラリ（`os`, `shutil`など）を活用することで、PC上の定型業務を自動化するスクリプトを簡単に作成できる。
- Pythonは、データ分析と自動化のための強力なライブラリ生態系を持つ。

**チェックポイント**:
- `Pandas`の`DataFrame`とExcelのスプレッドシートの類似点と相違点を説明できますか？
- Webサイトから特定の情報を定期的に取得したい場合、どのようなライブラリの組み合わせを検討しますか？
- あなたが日常的に行っているPC作業の中で、Pythonスクリプトで自動化できそうなものはありますか？

## 🔗 関連知識・発展学習
- [0132_SQL_Basic_Advanced.md](../../01_Development_Basic/013_Database_Basic/0132_SQL_Basic_Advanced.md): `Pandas`の操作は、SQLのデータ操作と多くの概念を共有しています。
- [19_AI_Machine_Learning](../../07_AI_Machine_Learning/): ここで学んだデータ処理のスキルは、機械学習のためのデータ前処理に直接つながります。
- **Jupyter Notebook / JupyterLab**: データ分析のプロセスをコード、実行結果、テキスト、グラフなどをまとめてインタラクティブに実行・記録できる、データサイエンティスト必須のツール。 