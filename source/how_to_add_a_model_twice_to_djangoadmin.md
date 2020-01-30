# 2.如何两次添加模型到Django admin 中

你需要将`Hero`模型添加两次到Django admin 中，一个作为常规管理区域，另外一个是只读区域
。
如果你重复注册相同的模型两次：
```Python
admin.site.register(Hero)
admin.site.register(Hero)
```
你将会获取以下错误：
```Python

raise AlreadyRegistered('The model %s is already registered' % model.__name__)
```
解决的方案是子类化`Hero`模型，作为ProxyModel:

```Python
# In models.py
class HeroProxy(Hero):

    class Meta:
        proxy = True

...
# In admin.py
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    list_display = ("name", "is_immortal", "category", "origin", "is_very_benevolent")
    ....


@admin.register(HeroProxy)
class HeroProxyAdmin(admin.ModelAdmin):
    readonly_fields = ("name", "is_immortal", "category", "origin",
        ...)
```
