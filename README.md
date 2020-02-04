# LAMP-container-from-CircleCI

## 概要
CircleCIのコンテナをベースに作った開発検証環境。<br>
（普通はこんな使い方をしないのだろうが、`circleci/php:〜-apache-node-browsers`はphpもnodejsも入っていて使いやすかったので。
）<br>
PHPの動作確認などにぜひ。。。

## 必要なもの
- docker
- docker-compose

## 使い方
1. リポジトリからダウンロードする。
1. 必要があれば`docker-compose.yml`や`apache2`の中を変更する。
    - 全部実行するのは時間がかかる。必要ないものはあらかじめ`docker-compose.yml`から削除してもよい。
1. `docker-compose build`を実行する。
1. lamp-container-from-circleci...のイメージができるのでそれを自身のプロジェクトの`docker-compose.yml`に指定して使用する。
    - `example-project-root`みたいな設定をして起動すれば、phpinfoが表示されるはず。
        - http://localhost:3074/
        - http://localhost:3073/
        - http://localhost:3072/
        - http://localhost:3071/
        - http://localhost:3070/

## その他
- [mkcert](https://github.com/FiloSottile/mkcert)などでローカル環境用SSLサーバ証明書を発行していれば、各サービスのvolumeに指定する事でhttpsも使用可能。
