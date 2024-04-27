---
title: 「ローカル環境 × Rails × Render」でデプロイ
tags:
  - Ruby
  - Rails
  - デプロイ
  - Render.com
private: false
updated_at: '2024-04-28T03:46:08+09:00'
id: 87fe50fa77af27afde16
organization_url_name: null
slide: false
ignorePublish: false
---

# はじめに

皆さま、こんにちは、すみ（@sumisumi2000） と申します。
2024 年 1 月 20 日より、オンラインプログラミングスクール RUNTEQ にて、Ruby on Rails を学習しています。

今回の記事では Docker を使わずに、ローカルで環境構築を行い、 Render.com でデプロイ＆データベース連携するまでの流れをまとめました。

https://render.com/

# 動作環境

- macOS Sonoma 14.4.1
- Ruby 3.3.0
- Rails 7.1.3.2

# ローカル環境構築

この記事では Docker を使用しません。
なので、ローカル環境（自身のパソコンの環境）で Ruby や Rails などをインストールする必要があります。

### 目次

1. [Homebrew のインストール](#homebrew-のインストール)
2. [rbenv のインストール](#rbenv-のインストール)
3. [Ruby のインストール](#ruby-のインストール)
4. [Rails のインストール](#rails-のインストール)
5. [PostgreSQL のインストール](#postgresql-のインストールデータベースが必要な方)

## 内容

### Homebrew のインストール

1. ルートディレクトリに移動

   - コマンド

     ```bash
     $ cd
     # 何も出力されないです
     ```

2. 移動できたか確認

   - コマンド

     ```bash
     $ pwd
     ```

     - ログ

       ```bash
       /Users/Macのコンピュータ名 # /Users/ の後ろが Mac のコンピュータ名になっていればOK
       ```

3. インストールされているか確認

   - コマンド

     ```bash
     $ brew -v # もしくは、 brew --version
     ```

     - ログ（インストール済みの場合）

       ```bash
       Homebrew 4.2.11 # バージョンが異なっていてもOK（数字が完全一致している必要はない）
       ```

   バージョンが表示されなければ下記の手順でインストールしてください。

#### インストール方法

1. https://brew.sh/ja/ にアクセス

2. 下の画像の赤い丸で囲まれたアイコンをクリック

   - コマンドをコピー
     ![Homebrew.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/7d9a4f27-ad83-4658-2b12-2a54b95b73cd.png)

3. ターミナルを開く

4. 貼り付けてコマンドを実行

   - コマンド
     `$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

インストールはこれで完了です。
ですが、バージョンを確認（ `brew -v` を実行）してもバージョンが出ないと思います。

- バージョンを確認するコマンド

  ```bash
  $ brew -v
  ```

  - ログ

    ```bash
    zsh: command not found:brew
    ```

ログがこのようになった方は下記の手順に沿ってパスを追加してください

#### パスの追加

1. ルートディレクトリに移動

   - コマンド

     ```bash
     $ cd
     # 何も出力されないです
     ```

2. `.zprofile` という名前のファイルを作成

   - コマンド

     ```bash
     $ touch .zprofile
     # 何も出力されないです
     ```

3. 環境変数を設定し、 `.zprofile` に保存

   - コマンド

     ```bash
     $ echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
     # 何も出力されないです
     ```

4. 設定を反映させる

   - `.zprofile` ファイルの内容を読み込んで実行させる

     - コマンド

       ```bash
       $ source ~/.zprofile
       # 何も出力されないです
       ```

5. インストールされてるか確認

   - コマンド
     ```bash
     $ brew -v # もしくは、 brew --version
     ```
     - ログ
       ```bash
       Homebrew 4.2.11 # バージョンが異なっていてもOK（数字が完全一致している必要はない）
       ```

#### 参考資料

https://zenn.dev/tet0h/articles/a92651d52bd82460aefb

### rbenv のインストール

- rbenv とは？
  - Ruby のバージョン管理ツールの一つです。
    - 複数の Ruby のバージョンを使用したり、使用する Ruby のバージョンを切り替えることができます。
  - 今回は最新の安定版の Ruby をインストールするために使用します。

1. インストールされてるか確認

   - コマンド

   ```bash
   $ rbenv -v
   ```

   - ログ

     - インストールされている場合

       ```bash
       rbenv 1.2.0 # バージョンが表示されていれば OK
       ```

     - インストールされていない場合

       ```bash
       zsh: command not found: rbenv
       ```

   - インストールされていない方は下記の手順でインストールしてください。

2. インストール

   - コマンド
     ```bash
     $ brew install rbenv ruby-build
     # ログがいっぱい出ます
     ```

3. 初期設定の設定

   - コマンド

     ```bash
     $ echo 'eval "$(rbenv init - zsh)"' >> ~/.zshrc
     # 何も出力されないです
     ```

4. 設定の反映

   - コマンド
     ```bash
     $ source ~/.zshrc
     # 何も出力されないです
     ```

5. インストールされたか確認

   - コマンド
     ```bash
     $ rbenv -v
     ```
     - ログ
       ```bash
       rbenv 1.2.0 # バージョンが表示されていればOK
       ```

<details><summary>バージョンが表示されない場合</summary>

バージョンが表示されない場合は以下の手順を試してみてください。
また、ターミナルを再起動することで設定が反映される場合もあります。

- 初期化

  ```bash
  $ rbenv init
  # Load rbenv automatically by appending
  # the following to ~/.zshr
  eval "$(rbenv init - zsh)"
  ```

- パスの追加

  ```bash
  $ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
  # 何も出力されないです
  ```

</details>

### Ruby のインストール

1. 安定版の確認

   - コマンド

     ```bash
     $ rbenv install -l
     ```

     - ログ

       ```bash
       3.0.6
       3.1.4
       3.2.3
       3.3.0
       jruby-9.4.6.0
       mruby-3.3.0
       picoruby-3.0.0
       truffleruby-23.1.2
       truffleruby+graalvm-23.1.2

       Only latest stable releases for each Ruby implementation are shown.
       Use `rbenv install --list-all' to show all local versions.
       ```

2. 最新版のバージョンを指定してインストール

   - コマンド

     ```bash
     $ rbenv install 3.3.0
     ```

     - ログ

       ```bash
       ruby-build: using openssl@3 from homebrew
       ==> Downloading ruby-3.3.0.tar.gz...
       -> curl -q -fL -o ruby-3.3.0.tar.gz https://cache.ruby-lang.org/pub/ruby/3.3/ruby-3.3.0.tar.gz
         % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                        Dload  Upload   Total   Spent    Left  Speed
       100 21.0M  100 21.0M    0     0  5605k      0  0:00:03  0:00:03 --:--:-- 5607k
       ==> Installing ruby-3.3.0...
       ruby-build: using libyaml from homebrew
       -> ./configure "--prefix=$HOME/.rbenv/versions/3.3.0" --with-openssl-dir=/opt/homebrew/opt/openssl@3 --enable-shared --with-libyaml-dir=/opt/homebrew/opt/libyaml --with-ext=openssl,psych,+
       -> make -j 8
       -> make install
       ==> Installed ruby-3.3.0 to /Users/Macのコンピュータ名/.rbenv/versions/3.3.0
       ```

   <details><summary>M3 チップの方でインストールができない場合</summary>

   以下の記事を参考にしてみてください。
   `rvm install 3.3.0 --with-openssl-dir=/usr/local/opt/openssl@1.1` は実行しない方がいいみたいです。

   https://github.com/rbenv/ruby-build/discussions/2364
   https://medium.com/@nelliemckesson/install-ruby-with-rvm-on-an-m3-pro-mac-784718bdb72a
   </details>

3. インストールできているか確認

   - コマンド

     ```bash
     $ rbenv versions
     ```

     - ログ

       ```bash
         system
       * 3.2.2 (set by /Users/Macのコンピュータ名/.rbenv/version)
         3.3.0 # 指定したバージョンがあればOK
       ```

4. インストールした Ruby のバージョンを使用するように設定

   - 切り替えるコマンドを実行

     ```bash
     $ rbenv global 3.3.0
     # 何も出力されないです
     ```

   - 切り替えられたか確認

     - コマンド

       ```bash
       $ rbenv versions
       ```

       - ログ

         ```bash
           system
           3.2.2
         * 3.3.0 (set by /Users/Macのコンピュータ名/.rbenv/version)
         # 指定したバージョンに * がついていればOK
         ```

    <details><summary>切り替えられない場合</summary>

   [`rails new` で rails アプリを作成](#rails-new-で-rails-アプリを作成)で作成したディレクトリに移動して`rbenv global` の代わりに `rbenv local` を試してみてください（M2 の Macbook で確認）

   - `global` → 全てのディレクトリ
   - `local` → 直下のディレクトリ

    </details>

#### 参考資料([rbenv のインストール](#rbenv-のインストール) & [Ruby のインストール](#ruby-のインストール) )

https://github.com/rbenv/rbenv

https://qiita.com/pien-neko/items/6134996d93596d2f6aef

https://gallard316.hatenablog.com/entry/2020/11/24/185634

### Rails のインストール

1. インストールされているか確認

   - コマンド

     ```bash
     $ rails -v
     ```

     - ログ

       - インストールされている場合
         ```bash
         Rails 7.1.3.2 # バージョンが表示されていれば OK
         ```
       - インストールされていない場合の例

         ```bash
         Rails is not currently installed on this system. To get the latest version, simply type:

             $ sudo gem install rails

         You can then rerun your "rails" command.
         ```

         ```bash
         rbenv: rails: command not found

         The `rails' command exists in these Ruby versions:
           3.2.2
         ```

   インストールされていなければ下記の手順でインストールしてください。

2. インストール

   - コマンド

     ```bash
     $ gem install rails
     # ログいっぱいでます
     ```

3. インストールされたか確認

   - コマンド

     ```bash
     $ rails -v
     ```

     - ログ

       ```bash
       Rails 7.1.3.2 # バージョンが表示されていれば OK
       ```

#### 参考資料

https://railsguides.jp/getting_started.html#railsのインストール

<details><summary>Rails のバージョンが確認できない場合</summary>

- エラー内容

  ```bash
  $ rails -v
  Rails is not currently installed on this system. To get the latest version, simply type:

      $ sudo gem install rails

  You can then rerun your "rails" command.
  ```

以下の記事を参考にしてみてください。
僕の場合は、[**rbenv のインストール**](#rbenv-のインストール)の「3. 初期設定の設定」をしていないと発生しました。

</details>

https://qiita.com/jnchito/items/e4872ff5c70a4c2219f1

### PostgreSQL のインストール（データベースが必要な方）

1. インストールされているか確認

   - コマンド

     ```bash
     $ psql --version
     ```

     - ログ

       - インストールされている場合

         ```bash
         psql (PostgreSQL) 14.11 (Homebrew) # バージョンが表示されていれば OK
         ```

       - インストールされていない場合（おそらく大体の人はこっちかも）

         ```bash
         zsh: command not found: psql
         ```

インストールされていなければ以下の手順でインストールしてください。

2. インストール

   - コマンド

     ```bash
     $ brew install postgresql
     # なんかいっぱいログでます
     ```

3. インストールされたか確認

   - コマンド

     ```bash
     $ psql --version
     ```

     - ログ

       ```bash
       psql (PostgreSQL) 14.11 (Homebrew) # バージョンが表示されていれば OK
       ```

---

# アプリの作成

ローカル環境構築が完了すれば、次はアプリの作成です。

カリキュラムではいつも `git clone` コマンドでアプリをローカルに作成しますが、今回は `rails new` コマンドで 0 からアプリを作成します。

### 目次

1. [`rails new` コマンドでアプリを作成](#rails-new-で-rails-アプリを作成)
2. [ローカルのデータベース作成](#ローカルのデータベース作成)
3. [ローカルで動作確認](#ローカルで動作確認)
4. [トップページ作成（必要な方のみ）](#トップページ作成確認用デプロイには必須ではない)

## 内容

### `rails new` で rails アプリを作成

1. アプリを作成するディレクトリに移動

   ルートディレクトリ直下ではなく`Workspace` などのディレクトリを作成して、そのディレクトリの中で `rails new` する方が管理が楽だと思います。

2. データベースに PostgreSQL を指定して`rails new` コマンドを実行

   - コマンド（ディレクトリ名は後から変更可能です）

     ```bash
     $ rails new myapp('ここは作成するアプリ名を入れてください') --database=postgresql
     # ログがいっぱい出ます
     ```

   - TailwindCSS を使用する方（未検証）

     - `--css=tailwind` オプションをつけてください

       ```bash
       $ rails new myapp --database=postgresql --css=tailwind
       ```

   - esbuild を使用する方（未検証）

     - `--javascript=esbuild` オプションをつけてください

       ```bash
       $ rails new myapp  --database=postgresql --javascript=esbuild
       ```

### ローカルのデータベース作成

1. 作成されたアプリのディレクトリへ移動

   - コマンド

     ```bash
     $ cd myapp
     # 何も出力されないです
     ```

2. データベース作成

   - コマンド

     ```bash
     $ rails db:create
     ```

   - ログ

     ```bash
     Created database 'myapp_development'
     Created database 'myapp_test'
     ```

### ローカルで動作確認

1. ローカルで挙動を確認するためにサーバーを起動

   - コマンド
     ```bash
     $ rails s
     ```
   - ログ
     ```bash
     => Booting Puma
     => Rails 7.1.3.2 application starting in development
     => Run `bin/rails server --help` for more startup options
     Puma starting in single mode...
     * Puma version: 6.4.2 (ruby 3.2.2-p53) ("The Eagle of Durango")
     *  Min threads: 5
     *  Max threads: 5
     *  Environment: development
     *          PID: 97975
     * Listening on http://127.0.0.1:3000
     * Listening on http://[::1]:3000
     Use Ctrl-C to stop
     ```

2. 確認

   - http://localhost:3000/ にアクセスして以下の画面が表示されていれば OK
     ![rails_new.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/85ed1cfe-6c46-1c43-a461-50bc6efb16bc.png)

### トップページ作成（確認用、デプロイには必須ではない）

この項目をしない場合はデプロイに成功しても以下のような画面が出ます。

トップページに何か表示したい方は以下の項目を実行してください。

![nothing_toppage.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/9be85606-fa88-3e4b-b9e5-932c436a79c0.png)

1. ジェネレーターの設定（必要な方のみ）

   - helper ファイル、test ファイル、ルーティングの記述がいらない方はしてください

     ```ruby:config/application.rb
     require_relative "boot"

     require "rails/all"

     # Require the gems listed in Gemfile, including any gems
     # you've limited to :test, :development, or :production.
     Bundler.require(*Rails.groups)

     module Myapp
       class Application < Rails::Application

         # 中略

         # ここから下5行を追記
         config.generators do |g|
           g.helper false             # helper ファイルを作成しない
           g.test_framework false     # test ファイルを作成しない
           g.skip_routes true         # ルーティングの記述を作成しない
         end
     	  # ここまで追記
       end
     end

     ```

2. コントローラとビューの作成

   - コマンド

     ```bash
     $ rails g controller staticpages top
     ```

     - ログ

       ```bash
             create  app/controllers/staticpages_controller.rb
             invoke  erb
             create    app/views/staticpages
             create    app/views/staticpages/top.html.erb
       ```

3. ルーティングを記述

   作成したトップページが http://localhost:3000/ にアクセスした時に表示されるようにルーティングを変更します。

   ```ruby:config/routes.rb
   Rails.application.routes.draw do
     root 'staticpages#top'
   end
   ```

4. ビューファイルを編集

   僕はデフォルトがあまり好きじゃないので編集します。
   ページにアクセスした時に表示される内容が変わるだけなのでお好みでどうぞ。

   ```html:app/views/staticpages/top.html.erb
   <h1>デプロイ成功です！</h1>
   <p>お疲れさまでした！！</p>
   ```

### エラーまとめ

- PostgreSQL をインストールせずに、 `rails new` した時に発生

https://qiita.com/youcune/items/5b783f7fde45d0fd4b35

- PostgreSQL サーバーを起動せずに、`db:create` した時に発生

https://qiita.com/mikan40/items/4784d6dde4d2ae9c7077

- `brew install libpq` をした後にアンインストールしたり、 `brew install postgresql` したり、合間に `rails new --database=postgresql` しなおしたりしてたら発生（あんまり覚えてない）

https://qiita.com/asami___t/items/cffec7972d871f203522

---

# アプリと GitHub の連携

ローカルにアプリを作成できたら、次は GitHub 上にリモートリポジトリを作成し、紐付けます。

今回デプロイ先で使用する Render は GitHub のリモートリポジトリをもとにデプロイするので、先ほど作成したローカルのアプリを GitHub に連携する必要があります。

### 目次

1.  [GitHub でリモートリポジトリを作成](#github-でリモートリポジトリを作成)
2.  [作成したアプリを GitHub と連携](#作成したアプリを-github-と連携)

## 内容

### GitHub でリモートリポジトリを作成

1. https://github.com/new にアクセスする

2. **「Repository name」** にアプリ名を入力

   - リポジトリ名は後から変更可能です

   ![GitHub_new.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/27cba82b-7f39-e7b8-4d07-e7e5bb29ab06.png)

3. 「**Create repositroy**」をクリックしてリポジトリを作成

   ![Create_repository.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/617e10ed-53d4-b1b2-e419-32590352cb59.png)

4. SSH をコピー（アイコンをクリック）

   ![SSH.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/e934a1d8-6a12-cd28-f645-23ff3aea75d4.png)

### 作成したアプリを GitHub と連携

1. リモートリポジトリの追加

   - `git remote add origin` の後にコピーした SSH をペースト

     ```bash
     $ git remote add origin git@github.com:[ユーザーネーム]/[アプリ名].git
     ```

   - 追加できたか確認

     - コマンド

       ```bash
       $ git remote -v
       ```

     - ログ

       ```bash
       origin	git@github.com:[ユーザーネーム]/[アプリ名].git (fetch)
       origin	git@github.com:[ユーザーネーム]/[アプリ名].git (push)
       ```

2. リモートリポジトリに必要ないファイルを `.gitignore` に追記

   ```ruby:.gitignore
   # See https://help.github.com/articles/ignoring-files for more about ignoring files.
   #
   # If you find yourself ignoring temporary files generated by your text editor
   # or operating system, you probably want to add a global ignore instead:
   #   git config --global core.excludesfile '~/.gitignore_global'
   # Ignore bundler config.
   /.bundle
   # 中略
   # Ignore master key for decrypting credentials and more.
   /config/master.key
   /app/assets/builds/*
   !/app/assets/builds/.keep
   # ここから下2行を追記
   .DS_Store
   /vendor/bundle
   # ここまで追記
   ```

3. commit & push

   - ステージング

     - コマンド

       ```bash
       $ git add -A
       # 何も出力されません
       ```

   - コミット

     - コマンド

       ```bash
       $ git commit -m 'init commit'
       # めっちゃログでます
       ```

   - プッシュ

     - コマンド

       ```bash
       $ git push -u origin main
       ```

       - ログ

         ```bash
         Enumerating objects: 96, done.
         Counting objects: 100% (96/96), done.
         Delta compression using up to 8 threads
         Compressing objects: 100% (80/80), done.
         Writing objects: 100% (96/96), 26.37 KiB | 4.40 MiB/s, done.
         Total 96 (delta 2), reused 0 (delta 0), pack-reused 0
         remote: Resolving deltas: 100% (2/2), done.
         To github.com:[ユーザーネーム]/[アプリ名].git
          * [new branch]      main -> main
         branch 'main' set up to track 'origin/main'.
         ```

4. 確認

GitHub の作成したリポジトリのページにアクセスして、コミットの内容が反映されていれば OK

### 参考資料

https://qiita.com/jojo232/items/6c3235e7efde85216129

---

# Render へのデプロイ

ローカルで作成したアプリと GitHub の連携が終われば、いよいよデプロイです！
今の状態は自分のパソコンからでしかアプリを触れない状態です。

（ `rails s` コマンドを実行して、 http://localhost:3000/ にアクセスする）

それをインターネットを通じて、誰でもアクセスできる状態にするのがデプロイです。

<details><summary>デプロイとは？ by ChatGPT3.5</summary>

> 「デプロイ」とは、開発したソフトウェアを実際に動作させる環境に配布し、適切に設定して使用可能な状態にすることを指します。このプロセスは、ソフトウェア開発の最終段階の一つであり、コードの開発が終わった後に行われます。
> 具体的には、以下のようなステップを含みます：
> **ビルド**：ソースコードから実行可能な形式（バイナリやバイトコードなど）を生成します。
> **テスト**：ビルドされたソフトウェアが予定通りに動作するかどうかを検証します。
> **リリース**：テストをパスしたソフトウェアをリリース用に準備します。
> **配布**：ソフトウェアをユーザーや顧客がアクセスする環境に配布します。これには、オンラインでのアップロードや物理的なメディアの配布が含まれることがあります。
> **運用**：ソフトウェアが問題なく動作するように監視し、必要に応じてメンテナンスやアップデートを行います。
> デプロイメントは、単純なウェブサイトの更新から、クラウドサービスや大規模なエンタープライズアプリケーションの展開に至るまで、さまざまな形態があります。また、デプロイメントの効率化と管理を目的とした様々なツールやプラクティス（CI/CD パイプラインなど）も存在します。

</details>

[Render.com](http://Render.com) は PaaS というデプロイを簡単にしてくれるサービスの一種です。

<details><summary>PaaS とは？ by ChatGPT3.5</summary>

> PaaS（Platform as a Service）は、「プラットフォームとしてのサービス」を提供するクラウドコンピューティングの一形態です。PaaS は、アプリケーションの開発、実行、管理に必要なプラットフォーム（ハードウェア、オペレーティングシステム、ミドルウェア、データベース、開発ツールなど）をインターネット経由で提供するサービスです。
> PaaS の主な利点は以下の通りです：
> **開発の迅速化**：開発者はインフラストラクチャの構築や管理に時間を割かずに、直ちにアプリケーション開発を始めることができます。
> **コスト削減**：物理的なサーバーやネットワーク機器を購入、運用、保守する必要がなくなるため、初期投資や運用コストが削減されます。
> **スケーラビリティ**：アプリケーションの需要に応じて、リソースを柔軟に調整することができます。使用した分だけ料金を支払うモデルが一般的です。
> **自動化と統合**：多くの PaaS 提供者は、データベース管理、バージョン管理、ワークフロー管理などの自動化ツールを提供し、開発プロセスを効率化します。
> **セキュリティとコンプライアンス**：PaaS プロバイダは、プラットフォームのセキュリティ対策やコンプライアンス要件の遵守を担当します。
> 例として、Google App Engine、Microsoft Azure、Heroku、IBM Cloud Foundry などがあり、これらのサービスを利用することで、開発者はアプリケーションの構築に集中でき、インフラストラクチャの詳細に関する心配から解放されます。

</details>

### 目次

1.  [アプリ側でデプロイ準備](#アプリ側でデプロイ準備)
2.  [Render 側で Web Service 作成＆デプロイ](#render-側で-web-service-作成＆デプロイ)

## 内容

### アプリ側でデプロイ準備

1. `bin` ディレクトリに `render-build.sh` という名前のファイルを作成し、以下の内容を記載します。

   ```ruby:bin/render-build.sh
   set -o errexit

   bundle install
   bundle exec rails assets:precompile
   bundle exec rails assets:clean
   ```

2. commit & push

   ```bash
   $ git add -A
   $ git commit -m 'お好みのコミットメッセージ'
   $ git push
   ```

### Render 側で Web Service 作成＆デプロイ

1. https://dashboard.render.com/ へアクセス

   - アカウント未作成 or 未ログインの方は GitHub アカウントでログイン

2. 「**New +**」というボタンを クリックし、 「**Web Service**」を選択

   ![Render_dashboard.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/a7a45312-89af-2b0c-d23b-23a162eb9143.png)

3. 「**Build and deploy from a Git repository**」にチェックが入っているのを確認し、「**Next**」をクリック

   ![new_Web_Service.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/eb2cee96-0d7a-f2a4-af47-d2e3be78567d.png)

    <details><summary>GitHub アカウントと Render を連携します（未連携の方のみ）</summary>

   a. 「**Connect GitHub**」のボタンをクリック
   ![Connect_GitHub.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/11e902aa-bf5e-520f-78c6-a1fbec3a3d33.png)
   b. 自分のアカウントを選択
   ![choose_GitHub_account.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/225b5098-393c-a8ff-d69d-e6b8d5ba4147.png)
   c. GitHub のどのリポジトリと連携するかを選択し、保存します。（お好みで）

   - 全部追加したくない人は「**Only select repositories**」を選択してください。
     ![Repository_access.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/09768dea-213d-08c0-ddda-85c97a7d0b2d.png)

    </details>

4. 「**Connect a repository**」の欄で先ほど作成したアプリのリポジトリを選択

   ![connect_a_repository.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/cf0466f4-50e8-7063-2660-8a2f15f8a2cc.png)

5. 項目を入力（編集する項目のみ記載しています）

   - **Name**
     - 自分のアプリ名を入力
   - **Region**
     - **「Singapore (Southeast Asia)**」を選択（日本から一番近い）
   - **Runtime**
     - 「**Ruby**」を選択
   - **Build Command**
     - 初期値では `bundle install; bundle exec rake assets:precompile; bundle exec rake assets:clean;` と入力されている項目
     - `./bin/render-build.sh` を入力（先ほど作成した `render-build.sh` を実行）
   - **Start Command**

     - 初期値では `bundle exec puma -t 5:5 -p ${PORT:-3000} -e ${RACK_ENV:-development}` と入力されている項目
     - `./bin/rails server` を入力

     ![Setting.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/06b64f4d-c944-d316-8774-a7ed900d56f7.png)

6. Free プランを選択

   ![Web_Service_plans.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/29293b2b-4b02-f99c-4112-016754f7e832.png)

7. 環境変数を追加

   **Environment Variables** の項目を設定

   - **RAILS_MASTER_KEY** の右側にあるフォーム（ プレースホルダーが`value` の部分）に自分のアプリの `config/master.key` ファイルの中身をコピー＆ペースト

   - 「**Add Environment Variable**」をクリックして、入力フォームを追加

     - 左側のフォーム（プレースホルダーが`NAME_OF_VARIABLE` の部分）に **`WEB_CONCURRENCY`** と入力
     - 右側のフォーム（プレースホルダーが `value` の部分）に **`2`** を入力

     ![environment_values.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/339315d0-242d-671f-a0f2-94cda9832177.png)

8. 左下の「Create Web Service」をクリックして作成

   ![Create_Web_Service.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/11894e3a-865b-a198-6188-f707eef054fa.png)

   - 下記のような画面が表示されれば、デプロイが始まっています。
     - 僕が作成してからデプロイ完了までにかかった時間：10 分弱
       ![deploying.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/8d29e2ab-f12b-0f89-1237-d324eb2eddf8.png)
   - 「**Cancel deploy**」のボタンが消えて、 「**…Building**」や「**…In progress**」の部分が 「**Live**」に切り替わればデプロイ完了です。
   - 左上の URL にアクセスすればトップページが表示されていると思います。
     ![deployComplete.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/637ac705-70ba-7195-216a-326303b887e7.png)
     - サイトにアクセスして確認
       ![success_deploy.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/54e1875b-7caf-a54c-b6a1-6e9e8b78cdcb.png)

### 参考資料

https://docs.render.com/web-services

https://docs.render.com/deploy-rails

---

# データベース連携（必要な方のみ）

:::note warn
Render が提供している無料の PostgreSQL は 90 日間限定です。
期限切れから、2 週間の猶予期間を過ぎるとデータベースが削除されます。
:::

https://docs.render.com/free#free-postgresql

本番環境でデータベースを使うには、データベースを作成し、デプロイしたサービスと連携する必要があります。

今回はわかりやすさを重視して、 Render が提供している PostgreSQL を使用します。

### 目次

1. [データベース作成](#データベース作成)
2. [データベース連携](#データベース連携)

## 内容

### データベース作成

1. https://dashboard.render.com/new/database にアクセス

2. 項目を入力

   - **Name**
     - ダッシュボードに表示される名前を入力
   - **Database（入力しなければ任意の値）**
     - データベースの名前を入力
   - **User（入力しなければ任意の値）**
     - ユーザー名を入力
   - **Region**（Web Service で選択したものと同じもの）
     - 「**Singapore (Southeast Asia)**」を選択
   - **Instance Type**
     - 「**Free**」を選択

   ![create_PostgreSQL.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/84bff7a8-d4bf-ceaf-820f-e58e7b717b08.png)

3. 左下の「**Create Database**」をクリックして作成

   ![Create_Database.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/f314b455-663a-b681-47b8-bba29a41c53d.png)

4. 「**Status**」が 「**Creating**」から「**Available**」になれば作成完了

   ![done_db.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/07bca871-5ab6-3769-7a31-9f81b4ce6745.png)

### データベース連携

#### Render 側での設定

1. https://dashboard.render.com/ から、作成したデータベースのページにアクセス

   ![dashboard_DB.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/2ee10bac-4337-1f3d-e60c-8da812debbb8.png)

2. 下にスクロールし、「**Connections**」の項目の中の「**Internal Database URL**」をコピー（アイコンをクリック）

   ![Internal_Database_URL.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/6a8c2669-cd0a-f51d-be0b-fe60d3bfe2e5.png)

3. [ダッシュボード](https://dashboard.render.com/)に戻り、作成したアプリのページにアクセス

   ![dashboard_App.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/a5794bfa-42da-770e-df24-45e23e4c539c.png)

4. 左側のメニューから「**Environment**」を選択し、「Add Environment Variable」をクリック

   ![environment_db.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/dcb28ea2-1fad-cbe8-1973-04a49c0ce26c.png)

5. 「**Key**」に `DATABASE_URL` 、「**Value**」に先ほどコピーした「**Internal Database URL**」の値をペーストし、 「**Save Canges**」で保存

   ![add_database_URL.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/ab9b5f04-9d2c-a9d8-8447-468befebc0f8.png)

#### アプリ側での設定

`bin/render-build.sh` に `bundle exec rails db:migrate` を追記

```ruby:bin/render-build.sh
set -o errexit

bundle install
bundle exec rails assets:precompile
bundle exec rails assets:clean
bundle exec rails db:migrate
```

##### テーブル作成（確認用、必要な方のみ）

1. scaffold コマンドを実行

   ```bash
   $ rails g scaffold User name:string
         invoke  active_record
         create    db/migrate/20240417084103_create_users.rb
         create    app/models/user.rb
         invoke  resource_route
          route    resources :users
         invoke  scaffold_controller
         create    app/controllers/users_controller.rb
         invoke    erb
         create      app/views/users
         create      app/views/users/index.html.erb
         create      app/views/users/edit.html.erb
         create      app/views/users/show.html.erb
         create      app/views/users/new.html.erb
         create      app/views/users/_form.html.erb
         create      app/views/users/_user.html.erb
         invoke    resource_route
         invoke    jbuilder
         create      app/views/users/index.json.jbuilder
         create      app/views/users/show.json.jbuilder
         create      app/views/users/_user.json.jbuilder
   ```

2. `rails db:migrate` を実行

3. `config/routes.rb` を以下のように編集

   ```ruby:config/routes.rb
   Rails.application.routes.draw do
     resources :users
     root 'users#index'
   end
   ```

4. ローカルで挙動を確認

   - ユーザーの CRUD ができれば OK
     ![local_users_index.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/37735bff-77de-59b1-a020-d9bb42bbef82.png)

   - commit & push
     ```bash
     $ git add -A
     $ git commit -m 'お好みのコミットメッセージ'
     $ git push
     ```

### 確認

プッシュが完了すると自動的にデプロイが始まります。
本番環境でもユーザーの CRUD ができれば OK です！

### 参考資料

https://docs.render.com/databases

https://docs.render.com/deploy-rails#deploy-manually

---

# 補足事項

### 確認用のファイルを削除

今回作成したトップページや `users` テーブルがいらない方は以下のコマンドを実行してください

#### `rails g controller staticpages top` の取り消し

こっちは簡単です、このコマンドを実行するだけで OK

```bash
$ rails destroy controller staticpages
# 中略
      remove  app/controllers/staticpages_controller.rb
      invoke  erb
      remove    app/views/staticpages
```

#### `rails g scaffold User name:string` の取り消し

1. ジェネレーターの取り消し

   ```bash
   $ rails destroy scaffold Users
   # 中略
         invoke  active_record
         remove    db/migrate/20240417084103_create_users.rb
         remove    app/models/user.rb
         invoke  resource_route
          route    resources :users
         invoke  scaffold_controller
         remove    app/controllers/users_controller.rb
         invoke    erb
         remove      app/views/users
         remove      app/views/users/index.html.erb
         remove      app/views/users/edit.html.erb
         remove      app/views/users/show.html.erb
         remove      app/views/users/new.html.erb
         remove      app/views/users/_form.html.erb
         remove      app/views/users/_user.html.erb
         invoke    resource_route
         invoke    jbuilder
         remove      app/views/users
         remove      app/views/users/index.json.jbuilder
         remove      app/views/users/show.json.jbuilder
         remove      app/views/users/_user.json.jbuilder
   ```

2. マイグレーションファイルが削除されてしまっているので復元

   - 矢印アイコンをクリック
     ![migration.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/75e8c77a-69d5-c21c-f479-d48496fcad92.png)
   - ファイルを復元をクリック
     ![restore_file.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2294602/55f411fa-827f-7a8d-cf7a-b039c6258884.png)

3. テーブルを削除するマイグレーションファイルを作成して、編集

   ```bash
   $ rails g migration DropUsers
   # 中略
         invoke  active_record
         create    db/migrate/20240417112425_drop_users.rb
   ```

   ```ruby:db/migrate/YYYYMMDDHHMMSS_drop_users.rb
   class DropUsers < ActiveRecord::Migration[7.1]
     def change
       drop_table :users do |t|
         t.string :name

         t.timestamps
       end
     end
   end
   ```

4. 作成したマイグレーションファイルを実行

   ```bash
   $ rails db:migrate
   == 20240417112425 DropUsers: migrating ========================================
   -- drop_table(:users)
      -> 0.0060s
   == 20240417112425 DropUsers: migrated (0.0062s) ===============================
   ```

5. `db/schema.rb` を確認し、テーブルが削除されていれば OK

### Render の tips

- Render は無料プランだと 15 分間アクセスがないと次のアクセスの際に読み込みがめっちゃ時間かかります。一応対策はできます。

https://qiita.com/ppputtyo/items/2ab44303dcfabeb7b23e

- 無料プランでコンソール使う方法

https://qiita.com/sami_0085/items/da68c9e0ef92a94f4d8a

### Homebrew のコマンド

- `brew list` インストールされてる一覧を表示
  ```bash
  $ brew list
  ==> Formulae
  autoconf	ca-certificates	gettext		git		icu4c		krb5		libyaml		lz4		m4		openssl@3	pcre2		pkg-config	postgresql@14	rbenv		readline	ruby-build
  ```
- `brew uninstall アンインストールするものの名前`

https://www.kikagaku.co.jp/kikagaku-blog/homebrew-install-howto/#i-9

# 終わりに

今回はローカルで環境構築を行ったので、次は Docker を使って環境構築を行ってみようと思います。
間違っている部分や気になる点があれば、コメントいただけると嬉しいです！

X もやっていますのでよろしければフォローお願いします！

https://twitter.com/sumisumi2000308
