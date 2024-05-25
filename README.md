# ETL Playground

Logstashを利用して、MySQLからOpenSearchへとデータをETLするプレイグラウンドです。
OpenSearchDashboardやJupiterNotebookを利用して、MySQLのレコードやOpenSearchに生成されたドキュメントを確認することもできます。


# 使用方法

## 初回使用時

利用前に環境変数、設定をコピーする必要があります。
コピー後のファイルを確認し、好みの設定に書き換えることができます。

```sh
cp -r .configs.example .config;
cp .env.example .env;
```

また、別途MySQLのJDBC Driverが必要です。
[こちらのURL](https://dev.mysql.com/downloads/connector/j/)から「Platform Independent」を選択してDLしたjarを利用することを想定しています。
`etl-playground/.configs/logstash/conf.d/`に配置し、`pipeline.conf`の`jdbc_driver_library`の調整をしてください。

## 起動

docker composeを利用して起動してください。

```sh
docker compose up -d
```

## データの投入

`docker compose up`による起動では`Logstash`によるデータ投入は行われません。
これはDBの初期化よりも早く`Logstash`が起動し、データの投入が失敗することを防ぐためです。

DBの起動完了後、以下のサンプルコマンドを利用して、`Logstash`コンテナを起動してデータの投入を行ってください。


```sh
docker compose run --rm logstash logstash -f /etc/logstash/conf.d/pipeline.conf
```

## データの確認

### OpenSearchDashboard

デフォルトでは以下のURLにアクセスすることで、OpenSearchDashboardsを使用できます。
`.env`の`DASHBOARDS_PORT`を変更することで、ポートの変更が可能です。

http://localhost:5601/app/home#/

### JupiterNotebook

デフォルトでは以下のURLにアクセスすることで、JupiterNotebookを使用できます。
`.env`の`NOTEBOOK_PORT`を変更することで、ポートの変更が可能です。

http://localhost:8888/lab/

