# LNPP (Linux + Nginx + PostgreSQL + PHP)環境構築用 Docker-Compose リポジトリ

## ■ 仕様

- Nginx コンテナ
- PHP コンテナ
  - Node.js
  - npm
  - Composer
  - Xdebug
- PostgreSQL コンテナ

## ■ ポート

| name       | port  |
| :--------- | :---- |
| Nginx      | 8080  |
| PostgreSQL | 15432 |
| XDebug     | 9012  |

## ■ 利用方法

1. `$ git clone git@github.com:hal-tech-club/docker_laravel.git`
2. `$ cd docker_laravel`
3. docker-compose.yml の`POSTGRES_DB`等を適当なものに変える
4. `$ docker-compose up --build`
5. 次回以降からは `$ docker-compose up -d`

### EX) VSCode 利用時… Xdebug を使用する設定

.vscode/launch.json を作成後以下を貼り付け。

```json
{
  "version": "0.2.0",
  "configurations": [{
    "name": "Listen for XDebug",
    "type": "php",
    "request": "launch",
    "port": 9012,
    "pathMappings": {
      "/var/www/html": "${workspaceFolder}"
    },
    "ignore": [
      "**/vendor/**/*.php"
    ]
  }]
}
```

実行ファイルは./html に配置する。

触るコンテナは基本的に php のみ。  
`composer` コマンド等はコンテナ内で行う。

```sh
# ターミナルでのコンテナへの接続方法
~/lnpp_env_docker $ docker-compose exec php ash
```

### ● SQLクライアントの接続設定

```dotenv
host: localhost:15432
user: postgres
password: postgres
database: sample
```

### ● Laravel の .env 用設定

```dotenv
DB_CONNECTION=pgsql
DB_HOST=postgres
DB_PORT=5432
DB_DATABASE=sample
DB_USERNAME=postgres
DB_PASSWORD=postgres
```

## ■Q&A

Q, VSCode のデバッグモードってどうやるの  
A, VSCode の`PHP Debug`という拡張機能をインストール後、上述の launch.json を用意した上で  
　 任意の php ファイルを開き任意の場所でブレークポイント(行数の左)を設定。  
　 VSCode の左サイドバーの虫と矢印のアイコンを押して`Listen for XDebug`を選択し再生ボタンを押す。  
　 任意のファイルのエンドポイントにブラウザでアクセスすると、VSCode の左サイドバーに変数の中身が表示される。
