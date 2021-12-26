# Dockerfileはテキストファイルであり、Dockerイメージを作り上げるために実行する命令をこのファイルに含めることができます。


# FROM命令はイメージビルドのためのベースイメージを設定する。FROM イメージ名:タグ名 で指定
# php | Docker Hub公式イメージをベースイメージとして利用する
FROM php:7.4-fpm-buster

# SHELL命令は何も指定しない場合は SHELL ["/bin/sh", "-c"] がデフォルト値となります(Linuxの場合)
# パイプ中のあらゆる段階でエラーがあれば失敗とするため、pipefailオプションを明示的に指定してます。
# -o オプションはオプションを設定するためのオプションです。
# -e オプションを定義しておくと、そのシェルスクリプト内で何らかのエラーが発生した時点で、それ以降の処理を中断できます。
# -u オプションを定義しておくと、未定義の変数に対して読み込み等を行おうとした際にエラーとなります。
# -x オプションを定義しておくと、実行したコマンドを全て標準エラー出力に出してくれます。
# おまじないと思ってもらえればokです。SHELL命令は必須ではないです。
SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]

# ENV命令はコンテナ内のサーバー環境変数を設定します。
ENV COMPOSER_ALLOW_SUPERUSER=1 \
  COMPOSER_HOME=/composer


COPY --from=composer:1.10 /usr/bin/composer /usr/bin/composer

# Debian系のパッケージ管理ツールは apt-get, apt とありますが、Dockerfile内で apt を実行するとCLIインターフェース向けではないと警告が表示されるため、apt-getを使用します。
# apt-get update インストール可能なパッケージの「一覧」を更新します。
# 実際のパッケージのインストール、アップグレードなどは行いません。
# apt-get -y install xxx Laravelのインストールに必要なパッケージをインストールします。
# 下記のパッケージをインストールしておけばokだと思います。
RUN apt-get update && \
  apt-get -y install git unzip libzip-dev libicu-dev libonig-dev && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/* && \
  docker-php-ext-install intl pdo_mysql zip bcmath


COPY ./php.ini /usr/local/etc/php/php.ini


WORKDIR /work