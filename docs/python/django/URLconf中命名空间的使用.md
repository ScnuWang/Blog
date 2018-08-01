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

