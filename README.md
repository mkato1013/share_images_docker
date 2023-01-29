# share_images
## アルバムを共有するアプリ

### 仕様(適宜追加)
- 画像をアルバムに登録
- アルバム内で画像名の有無を登録できる
- 画像名、登録日時で検索できる
- 並び替え可能
- ユーザー登録可能
- フォロー可能
- 画像にいいね可能
- 管理者ユーザー作成

### テーブル(適宜追加)
- 画像
- ユーザー
- アルバム
- いいね
- フォロー管理

## Usage

- DBコンテナ
https://github.com/mkato1013/mariaDB
をクローンして、コンテナ起動。

```
$ git clone https://github.com/mkato1013/mariaDB.git
$ cd mariadb
$ docker-compose up -d
```

### shareimages_localのDB作成
- DB構築

```
CREATE DATABASE IF NOT EXISTS shareimages_local CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```
- ユーザー作成、shareimages_localへの権限付与

```
GRANT ALL ON shareimages_local.* TO shareimages@'%' IDENTIFIED BY 'HhmZGK2mG6';
```

### ソースコードClone
```
$ cd share_images_docker
$ git clone https://github.com/mkato1013/share-images.git
```

### コンテナbuild
```
$ docker-compose build
```

### コンテナ起動
```
$ docker-compose up -d
```

### Laravelの環境構築
```
$ docker exec -it share-images bash
$ chown -R www-data:www-data /var/log/laravel
$ composer install
```

### DBのマイグレーション
```
$ php artisan migrate
$ php artisan db:seed
```

### 接続
http://localhost:20181

開発環境の場合は localで指定している箇所をdevに変更