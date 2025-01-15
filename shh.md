# GitHub アカウントへの新しい SSH キーの追加

- 参考 [GitHub SSh](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account?platform=mac&tool=webui)

## SSH について

- リモートのサーバーやサービスに接続し、認証を受けられます。
- アクセスのたびにユーザー名と personal access token を入力しなくても、GitHub に接続できます
- SSH キーを使ってコミットに署名することもできます

### 前提条件

- GitHub は 2022 年 3 月 15 日に古いセキュリティで保護されていないキーの種類を削除することでセキュリティを強化しました。
- DSA キー (`ssh-dss`) はサポートされなくなりました。
- Mac OS でのターミナル操作でのやり方です

## SSH キーを確認の仕方

1, ターミナルを開き、`ls al ~/.ssh`

- `al` で隠しファイルが見られます

```
ls al ~/.ssh

```

2, ディレクトリの一覧から、公開 SSH キーをすでに持っているか確認します。 既定では、GitHub でサポートされている公開鍵のファイル名は次のいずれかです。

- id_rsa.pub

- id_ecdsa.pub

- id_ed25519.pub

## SSH キーを生成

1, ターミナルを開く

```

# your_email@example.com は GitHub メール アドレスに置き換えます。

ssh-keygen -t ed25519 -C "your_email@example.com"
```

これにより、指定したメールアドレスをラベルとして使って新しい SSH キーが作成されます。

2, Enter キーを押して既定のファイルの場所をそのまま使うことができます

- `SSH キーを作成した場合、ssh-keygen から別のキーの書き換えを求められる場合があることに注意してください`

その場合は、カスタム名の SSH キーを作成することをお勧めします。

そのためには、デフォルトのファイル位置をタイプし、id_ALGORITHM をカスタムキー名で置き換えます。

```
> Enter a file in which to save the key (/Users/YOU/.ssh/id_ALGORITHM): [Press enter]

```

3, プロンプトが表示されたら、セキュアなパスフレーズを入力します。

```
# パスフレーズを入力してください（パスフレーズがない場合は空白）:
> Enter passphrase (empty for no passphrase):


# 同じパスフレーズをもう一度入力してください:
> Enter same passphrase again:
```

## SSH キーを ssh-agent に追加する

- 秘密鍵が他に盗まれた場合がある為`ssh-agent` を追加すると秘密鍵を保護してくれます。

1, バックグラウンドで ssh-agent を開始します。

```
$ eval "$(ssh-agent -s)"
> Agent pid 59566
```

2, `macOS Sierra 10.12.2` 以降を使っている場合, `~/.ssh/config` ファイルを修正して、キーが ssh-agent に自動的に読み込まれ、パスフレーズがキーチェーンに格納されるようにする必要があります。

- まず、`~/.ssh/config` ファイルが既定の場所に存在するかどうかを調べます。

```
$ open ~/.ssh/config

# ファイルがない
> The file /Users/YOU/.ssh/config does not exist.
```

- ファイルがない場合は、ファイルを作成します。

```
touch ~/.ssh/config
```

-　もう一度開く

```
open ~/.ssh/config

```

`config`が出てくるので下記をコピーし入りつける

```
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519

```

3, SSH 秘密鍵を ssh-agent に追加して、パスフレーズをキーチェーンに保存します。 キーを別の名前で作成した場合、または別の名前の既存のキーを追加する場合は、コマンドの id_ed25519 を秘密キー ファイルの名前に置き換えます。

```
ssh-add --apple-use-keychain ~/.ssh/id_ed25519

```

- パスフレーズを求められます

```
Enter passphrase for /youUser/.ssh/id_ed25519:

```

パスフレーズを入力して登録完了

次に GitHub で SSH キーの追加をします。

## GitHub アカウントへの新しい SSH キーの追加

```
$ pbcopy < ~/.ssh/id_ed25519.pub
# Copies the contents of the id_ed25519.pub file to your clipboard

```

コピーしたら

GitHub > Settings > SSH andn GPG keys に移動して

`New SSH key` をクリックして

タイトル入力

Key type を選択します

コピーし.ssh を貼り付けましょう

## SSH をテストします

[SSH をテスト](https://docs.github.com/ja/enterprise-cloud@latest/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection)

```
ssh -T git@github.com
# Attempts to ssh to GitHub Enterprise Cloud

```

### 成功したら

```
Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.

```

### 失敗したら

```
> The authenticity of host 'github.com (IP ADDRESS)' can't be established.
> ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
> Are you sure you want to continue connecting (yes/no)?

```

と警告されます。

## 参考 URL

[動画](https://www.youtube.com/watch?v=5Vndfy68yho)

[GitHub SSH](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account?platform=mac&tool=webui)

[SSH をテスト](https://docs.github.com/ja/enterprise-cloud@latest/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection)
