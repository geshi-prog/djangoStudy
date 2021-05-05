# 環境構築方法
## 別リポジトリにするには
https://chat-rate.com/it/1789/
## Clone
まずは、クローンします。
```
git clone https://github.com/geshi-prog/djangoDocker.git
```
## DBの名前を変更
docker-compose.ymlのMYSQL_DATABASEのdbNameを<作成したいDBの名前>に変更します。
## docker compose
```
cd djangoDocker
docker-compose run python django-admin.py startproject <作成したいプロジェクト名> .
```
### settings.pyを変更
まずは、下記を追加します。
```
import os
import pymysql

# connect mysql
pymysql.install_as_MySQLdb()
```
次にDBを修正します。
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': '<作成したいDBの名前>',
        'USER': 'user',
        'PASSWORD': 'password',
        'HOST': 'db',
        'PORT': '3306',
    }
}
```
次に日時を修正します。
```
LANGUAGE_CODE = 'ja'

TIME_ZONE = 'Asia/Tokyo'
```
最後に静的ファイルを読み込むために下記を追記します。
```
STATIC_ROOT = '/static'
```
## マイグレーションの実施
```
docker-compose run python ./manage.py makemigrations
docker-compose run python ./manage.py migrate
```
## 管理者の設定
```
docker-compose run python ./manage.py createsuperuser
```
## 静的ファイルの設定
```
docker-compose run python ./manage.py collectstatic
```
## ページに遷移して確認
http://localhost:8000/admin
に遷移してアクセスできれば完了です。
