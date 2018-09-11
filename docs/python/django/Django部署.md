Django 2.1.1 

Python 3.5.2

1. 上传django项目到服务器，保证python manage.py runserver能正常运行并浏览器访问

[`django.contrib.staticfiles`](https://docs.djangoproject.com/en/2.0/ref/contrib/staticfiles/#module-django.contrib.staticfiles) 提供便捷管理命令，用于在单个目录中收集静态文件，以便您可以轻松地为它们提供服务。

1. 将[`STATIC_ROOT`](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-STATIC_ROOT)设置设置为您要从中提供这些文件的目录，例如：

   ```
   STATIC_ROOT = "/var/www/example.com/static/"
   ```

2. 运行[`collectstatic`](https://docs.djangoproject.com/en/2.0/ref/contrib/staticfiles/#django-admin-collectstatic)管理命令：

   ```
   $ python manage.py collectstatic
   ```

   这会将静态文件夹中的所有文件复制到 [`STATIC_ROOT`](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-STATIC_ROOT)目录中。

3. 使用您选择的Web服务器来提供文件。[部署静态文件](https://docs.djangoproject.com/en/2.0/howto/static-files/deployment/)涵盖了[静态文件的](https://docs.djangoproject.com/en/2.0/howto/static-files/deployment/)一些常见部署策略

```
no such item for Cursor instance
```





do-release-upgrade



安装Nginx

apt install nginx

安装uwsgi

pip install uwsgi --upgrade



命令方式：

uwsgi --http :8000 --chdir /root/GeekparityWebsite  --module GeekParitySystem.wsgi





https://uwsgi-docs.readthedocs.io/en/latest/tutorials/Django_and_nginx.html

常用命令：

nginx

启动：service nginx start

重启：service nginx reload

uwsgi

supervisor

启动supervisord ：supervisord -c /etc/supervisord.conf

重启：supervisorctl reload

关闭应用：supervisorctl stop 应用名

开启应用：supervisorctl start 应用名

重启应用：supervisorctl restart 应用名



MongoDB

启动：service mongod start

关闭：service mongod stop

重启：service mongod restart

使用：mongo --host 127.0.0.1:27017