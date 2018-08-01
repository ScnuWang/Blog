注：在给URL配置了命名空间之后，在任何地方使用url路径的名称时，都要添加命名空间的名称。

### 正确写法

```
urlpatterns = [
    path('', home, name='home'),
    path('admin/', admin.site.urls),
    path('product/', include(('product.urls','product'),namespace='product')),
]
```

### 错误写法

```
urlpatterns = [
    path('', home, name='home'),
    path('admin/', admin.site.urls),
    path('product/', include('product.urls',namespace='product'))，
]
```

### 错误写法会导致的问错误提示

```
  File "D:\ParitySystem\ParitySystem\urls.py", line 24, in <module>
    path('product/', include('product.urls',namespace='product')),
  File "D:\ParitySystem\venv\lib\site-packages\django\urls\conf.py", line 39, in include
    'Specifying a namespace in include() without providing an app_name '
django.core.exceptions.ImproperlyConfigured: Specifying a namespace in include() without providing an app_name is not supported. Set the app_name attribute in the included module, or pass a 2-tuple containing the list of patterns and app_name instead.
```

### 代码中如何使用命名空间

```
if login_form.is_valid():
	auth.login(request,login_form.cleaned_data['user'])
	return redirect(reverse('product:product_list',args=[]))
```

### html模板中使用

```
<div class="col-md-3">
    <a href="{% url 'product:product_detail' product.original_id  %}">
        <div class="panel panel-default" >
            <div class="panel-heading" style="background-color: #ffffff">
            <img src="{{ product.project_picUrl }}" alt="Geek 比价" style="width: 195px;height: 195px;" class="img-rounded center-block">
            </div>
            <div class="panel-body text-center">
            <p>{{ product.project_name }}</p>
            <p>{{ product.project_desc|truncatechars:10 }}</p>
            <p style="color: #c00000">{{ product.project_price }}</p>
            </div>
        </div>
    </a>
</div>
```

