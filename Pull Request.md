# Pull Request のやり方

ブランチが複数ある場合それを(結合)マージするのに必要です。

最初に`main`ブランチを作成しているので、追加でブランチを作成します。

## ブランチを作成

1.  リポジトリをクローンします

    ```
    git clone https://github.com/username/repository.git

    ```

2.  新しいブランチを作成します

    ```
    git checkout -b feature/branch-name

    ```

3.  変更を加え、コミットします

    ```
    git add .

    git commit -m "Add new feature or fix"

    ```

4.  リモートリポジトリにプッシュします。

    ```
    git push origin feature/your-feature

    ```

## GitHub で Pull Request を作成

1. GitHub ページ > `Pull requests` > New pull request をクリック

2. `base` と `compare`が選択できます。

- base を`main`で

- compare を`feature/your-feature` で選択します

3. ファイル内容が問題なければ `Create pull request`をクリック
