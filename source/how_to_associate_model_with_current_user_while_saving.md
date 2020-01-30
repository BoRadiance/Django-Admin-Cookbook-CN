# 2. 保存时，如何将当前的用户和模型关联起来
`Hero` 模型有以下在字段
```Python
added_by = models.ForeignKey(settings.AUTH_USER_MODEL,
        null=True, blank=True, on_delete=models.SET_NULL)
```
当对象通过admin创建时，你需要`added_by`字段自动设置为当前用户。你可以这样做：
```Python
def save_model(self, request, obj, form, change):
    if not obj.pk:
        # Only set added_by during the first save.
        obj.added_by = request.user
    super().save_model(request, obj, form, change)
```
如果你想始终保存当前用户，就可以这样做:
```Python
def save_model(self, request, obj, form, change):
    obj.added_by = request.user
    super().save_model(request, obj, form, change)
```
如果你还想隐藏`add_by`字段，让它不显示在更改表单上，可以这样做：
```Python
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    exclude = ['added_by',]
```
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/changeview_readonly.png)
