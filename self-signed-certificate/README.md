オレオレ証明書ハンズオン
=====

## オレオレ証明書を生成してみる

参考情報）
https://katekichi.hatenablog.com/entry/2017/06/14/docker_for_mac_%2B_nginx_%2B_%E3%82%AA%E3%83%AC%E3%82%AA%E3%83%AC%E8%A8%BC%E6%98%8E%E6%9B%B8%E3%81%A7%E3%83%AD%E3%83%BC%E3%82%AB%E3%83%ABSSL%E7%92%B0%E5%A2%83%E3%82%92%E4%BD%9C%E3%81%A3%E3%81%9F

```shell
$ brew list openssl # opensslが入っているか確認
$ openssl genrsa 2048 > server.key
$ openssl req -new -key server.key > server.csr
```

```
# こんなシェル出力になると思うが、適当に入力していく
# パスフレーズはなしで

You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:JP
State or Province Name (full name) [Some-State]:XXXX
Locality Name (eg, city) []:XXXX
Organization Name (eg, company) [Internet Widgits Pty Ltd]:XXXX
Organizational Unit Name (eg, section) []:XXXX
Common Name (e.g. server FQDN or YOUR name) []:localhost
Email Address []:hoge@localhost

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []: <--- パスワードは空白でOK
An optional company name []:
```

```shell
$ openssl x509 -in server.csr -days 100 -req -signkey server.key > server.crt
```

こんなディレクトリ構成になっているはず

```
$ ls *
Dockerfile README.md  app.conf

html:
index.html

keys:
server.crt server.csr server.key
```

## Dockerに入れてみる

```shell
docker build --tag webapp .
docker run --name webapp -p 443:443 -p 80:80 webapp
```

## アクセスしてみる

`https://localhost`

## オレオレ証明書を信頼してみる

1. server.crt をダブルクリックしてキーチェーンアクセスに取り込み
1. キーチェーンアクセスを開いて、localhostで検索、ダブルクリック
1. 信頼設定を変更して保存

![キーチェーンアクセスで証明書を特定](https://user-images.githubusercontent.com/1808432/175501082-7f95b638-d174-4068-9513-e23fa271ea72.png)

![信頼設定の変更](https://user-images.githubusercontent.com/1808432/175501028-2655e533-b327-41d2-bb30-9ee059d4207f.png)

## アクセスしてみる

`https://localhost`
