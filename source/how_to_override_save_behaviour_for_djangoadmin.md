# 3.如何重写Django admin的保存行为
`ModelAdmin`有一个`save_model`方法，这个方法用于创建和更新模型对象，你可以自定义admin保存行为。

`Hero`模型有下面的字段：
```Python
added_by = models.ForeignKey(settings.AUTH_USER_MODEL,
        null=True, blank=True, on_delete=models.SET_NULL)
```
如果你想`Hero`更新的时候，总是保存当前用户，你可以这样做：
```Python
def save_model(self, request, obj, form, change):
    obj.added_by = request.user
    super().save_model(request, obj, form, change)
```

