由于Django默认不支持MongoDB，所以并没有直接提供MongoDB的backend。但是我们可以借助第三方的库（mongoengine），这让我想到了Java中的，Mybatis和Spring。

mongoengine的[文档](http://docs.mongoengine.org/)，安装这些都在里面有说明，我这里就不多说了，这里重点说一下，怎么在Django里面配置。

说明：本次文章主要配置MySQL以及MongoDB两个数据库，在配置多个数据库，依葫芦画瓢应该不难。

### settings.py文件中的配置

默认使用mysql，

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'geekparity',
        'USER': 'root',
        'PASSWORD': 'admin',
        'HOST': 'localhost',
        'PORT': '3306',
    },
    'mongo_db':{
       'ENGINE': None,
    },
}


from mongoengine import connect
connect('wangyi',host='127.0.0.1',port = 27017)  # 连接的数据库名称

# 多数据库路由配置
DATABASE_ROUTERS = ['path.dbrouter.AuthRouter',]

```

### dbrouter.py文件配置

直接在官方文档中找到[以下代码](https://docs.djangoproject.com/en/2.0/topics/db/multi-db/)，做简要修改即可，我们这里设置的是默认使用default数据库，就是mysql数据库，product模块中使用MongoDB数据库

```
class AuthRouter:
    """
    A router to control all database operations on models in the
    auth application.
    """
    def db_for_read(self, model, **hints):
        """
        Attempts to read auth models go to auth_db.
        """
        if model._meta.app_label == 'product':
            return 'mongo_db'
        return 'default'

    def db_for_write(self, model, **hints):
        """
        Attempts to write auth models go to auth_db.
        """
        if model._meta.app_label == 'product':
            return 'mongo_db'
        return 'default'

    def allow_relation(self, obj1, obj2, **hints):
        """
        Allow relations if a model in the auth app is involved.
        """
        if obj1._meta.app_label == 'product' or \
           obj2._meta.app_label == 'product':
           return True
        return 'default'

    def allow_migrate(self, db, app_label, model_name=None, **hints):
        """
        Make sure the auth app only appears in the 'auth_db'
        database.
        """
        if app_label == 'product':
            return db == 'product'
        return 'default'
```

### models.py文件配置

这里主要强调的是，需要指定要使用哪个collection

```
from mongoengine import Document,StringField,IntField,DecimalField,DateTimeField
# Create your models here.
class ProductModel(Document):
    # 指定collection
    meta={'collection': 'projects'}
    # 原始产品编号
    original_id = StringField()
    # 产品名称
    project_name = StringField()
    # 产品价格
    project_price = DecimalField()
```

