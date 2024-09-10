# Laravleの環境をDockerで構築するテンプレート

## このリポジトリの説明
このリポジトリは、DockerでLaravelの環境を構築するものになります。  
コンテナ内でgit操作をし、githubにssh接続してもらう予定です。 

## 前提条件
- Dockerでの開発ができる環境が整っている
- 最低限のコマンド操作(cd, lsなど)を扱える

## はじめかた
1. まず、`git clone`をしていただき、自身のPCに環境を用意してください。
```
git clone https://github.com/Rei1449/docker-laravel10-template.git
```
2. srcディレクトリと.sshディレクトリをcompose.ymlと同じディレクトリ内に作成してください。
```
/
|- compose.yml
|- .env.example
|- .gitignore
|- docker/
|- src/ # new
|- .ssh/ # new
```
3. .env.exampleをコピーし、.envファイルを書く。中身の設定は適宜行う。下記は例。
```.env
# ローカルのポート番号のため、被らないように注意
WEB_PORT=80
DB_PORT=3306
PMA_PORT=8080 # phpmyadminの設定をしたら記載

# ここはどんな値でも構わない
DB_NAME=blog
DB_USER=dbuser
DB_PASSWORD=testpass
DB_ROOT_PASSWORD=testpass
```
4. `docker/php/Dockerfile`のgitに関する記述を自分用に置き換えてください。
```dockerfile
~~~
&& git config --global user.name "githubの名前" \
&& git config --global user.email "githubに登録したメールアドレス" \
~~~
```
5. `compose.yml`があるディレクトリに移動し、`docker compose build`でDockerfileからDockerイメージをビルドします。
8. buildしたら下記のコマンドでdockerコンテナを起動し、コンテナに入りLaravelプロジェクトを作成する。
```
docker compose up -d
docker compose exec app bash
# dockerコンテナ内に入れたら
composer create-project laravel/laravel --prefer-dist . "10.*"
```
4.  プロジェクトが作成できたら、`localhost:80`に接続しLaravelのWelcome Pageが表示されるかを確認してください。
5.  次に、sshの設定を行います。
6.  `docker compose exec app bash`でdockerコンテナ内に入ってください。（すでに入っている場合は大丈夫です！）
7.  コンテナ内に入ったら、`ssh-keygen -t rsa`で公開鍵と秘密鍵を作成します。全てEnterで大丈夫です！
8.  2で作成した.sshディレクトリ配下に2つのファイルができているはずなので、そのうちの`id_rsa.pub`を開き、中身を全てコピーしてください。
以降の手順はカリキュラム解答のgit編と同じなので、それを見ていただけると助かります。
