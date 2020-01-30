# 3.admin中如何只能创建一个对象
UMSRA管理员要求你将类别的数量限制为一个。 他们希望每个实体都属于同一类别。

你可以通过下面这个做：
```Python
MAX_OBJECTS = 1

def has_add_permission(self, request):
    if self.model.objects.count() >= MAX_OBJECTS:
        return False
    return super().has_add_permission(request)

```
一旦创建一个对象，就会隐藏添加按钮，你可以设置MAX_OBJECTS设置为任何值，以确保最多可以容纳超过MAX_OBJECTS个对象。