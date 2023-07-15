# 使い方
SSL化非対応
## ホスト名の設定
`/etc/hosts`に以下を追加(管理者権限)

```
172.0.0.1 web1.local
172.0.0.1 web2.local
```

## コンテナ起動
`proxy-server`、`web1`、`web2`のディレクトリ内で以下を実行

```
docker-compose up -d
```

## コンテナを新規作成する場合
`docker-compose.yml`に以下を追加

`environment`の`VIRTUAL_HOST: `は[これ]( #ホスト名の設定 )と同じ作業をする

`networks`の`name: `は`proxy-server/docker-comfig.yml`で指定したものと同じにする

```
services:
  example:
    image:
    environment:
      VIRTUAL_HOST: 'example.local'

networks:
  default:
    external: true
    name: example
```

# なにができるの？
- proxy-serverがクライアントとWEBサーバーの間に入り、代理で通信する。複数コンテナとできる。
- proxy-serverを経由することで既存コンテナが簡単にSSL化できる。
- 新規作成するコンテナでも[これ]( #コンテナを新規作成する場合)の作業でプロキシサーバー経由にできる。

# なにが起こるの？
- コンテナ起動後、ブラウザでlocalhostにアクセスすると503が返ってくる。
- `web1.local`と`web2.local`がプロキシサーバー経由(proxy-netコンテナ)でアクセスできる。
- web1.localにアクセスすると、php7.2-apacheが実行されているコンテナにアクセスできる。
- web2.localにアクセスすると、nginxが実行されているコンテナにアクセスできる。
