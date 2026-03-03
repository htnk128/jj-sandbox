# jj-sandbox

[jj (Jujutsu)](https://github.com/jj-vcs/jj) を試すためのサンドボックスリポジトリです。

## jj とは

次世代のバージョン管理システム（VCS）です。

### 名前の読み方

柔術（じゅうじゅつ）」なのか「呪術（じゅじゅつ）」なのかコミュニティでも議論されています（[#3021](https://github.com/jj-vcs/jj/discussions/3021)）。
（どっちでもいいけど）発音についてはどちらなのかはっきりしてほしいところです。
ここでは **jj** と呼びます。

### 特徴

#### Git 互換

既存の Git リポジトリと連携できます。

#### Git よりもシンプル

**ステージング不要**

| | コマンド |
|---|---|
| Git | `git status` → `git add hoge` → `git commit -m "fix: ほげほげ"` |
| jj | `jj st` → `jj describe -m "fix: ほげほげ"` |

-> `git add` が不要なため、add 忘れが発生しません。変更は自動追跡されます。

**コミット直後に追加し忘れた**

```bash
# Git
git add .
git commit --amend

# jj
jj commit --amend
# または
jj amend
```

-> jj は履歴を後から直す前提の設計なので、rebase 地獄から解放されます。

**途中の作業を保存したい**

```bash
# Git
git stash
# または中途半端にコミットする

# jj
# 何もしなくて良い
```

作業途中でも履歴に含まれる設計になっています。

## セットアップ

### 1. インストール
```bash
brew install jj
```

### 2. Gitと同様にユーザ名とメールアドレスを設定

```bash
jj config set --user user.name "$(git config --global --list | grep -E '^user\.name=' | cut -d= -f2)"

jj config set --user user.email "$(git config --global --list | grep -E '^user\.email=' | cut -d= -f2)"

jj config list | grep -E "^user\.(name|email) ="
```

### 3. リポジトリのクローン

```bash
git clone git@github.com:htnk128/jj-sandbox.git
cd jj-sandbox
```

### 4. jj の初期化（Git との共存モード）

```bash
jj git init --colocate
```

`--colocate` オプションを指定することで、既存の `.git` ディレクトリをそのまま利用しつつ jj を初期化します。Git と jj を同じリポジトリで併用できます。
