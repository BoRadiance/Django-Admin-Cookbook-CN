# 4.怎么把默认的apps 从Django admin移除

Django 会把`django.contrib.auth `添加到 `INSTALLED_APPS`，这意味着，*User*和*Groups* 模型会被自动添加到admin里面
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/remove_default_apps.png)

如果你想移除它的话， 你需要去取消注册掉他们
```Python
from django.contrib.auth.models import User, Group
admin.site.unregister(User)
admin.site.unregister(Group)
```
当你做完这些改变之后，你的admin就像下面这样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/remove_default_apps_fixed.png)