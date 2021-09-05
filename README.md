# Docker Compose で PlantUMLサーバー（on Jetty）を動作させて Nginxでプロキシとキャッシュをする

## 概要

題名の通り

## 使い方

このリポジトリをクローンして`docker compose up`コマンドを実行して起動するだけ。

## 詳しく

PlantUMLサーバーはコンテキストパス`plantuml`に配置している。  
変更したい場合は`docker-compose.yml`の9行目を変更し、Nginxの`default.conf`のプロキシパスも変更する。

```yml
version: '3.8'
services:
  plantuml-server:
    container_name: plantuml-server
    build:
      context: plantuml-server/build
      dockerfile: Dockerfile
      args:
        context_path: plantuml # ←ここ
    restart: always
    ports:
      - "8080:8080"
```

```
  location ^~ /plantuml {
    proxy_pass        http://plantuml-server:8080/plantuml; # ←ここ
    proxy_cache       my_cache;
    proxy_set_header  Host                $host;
    proxy_set_header  X-Real-IP           $remote_addr;
    proxy_set_header  X-Forwarded-Host    $host;
    proxy_set_header  X-Forwarded-Server  $host;
    proxy_set_header  X-Forwarded-For     $proxy_add_x_forwarded_for;
    add_header        X-Nginx-Cache       $upstream_cache_status;
  }
```

## ライセンス

MIT
