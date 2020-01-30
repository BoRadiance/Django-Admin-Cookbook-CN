# 1. 如何在列表页展示计算字段
你有一个`Origin`模型 的admin，向下面这样：
```Python
@admin.register(Origin)
class OriginAdmin(admin.ModelAdmin):
    list_display = ("name",)
```
除了名称，我们还希望显示每个`Origin`的`Hero`英雄数量和`Villain`反派数量，这些不是`Origin`上的DB字段。 您可以通过两种方式执行此操作。

## 1.1 在模型上添加方法
你可以为你的`Origin`添加2个方法，像下面这样：
```Python
def hero_count(self, obj):
    return obj.hero_set.count()

def villain_count(self, obj):
    return obj.villain_set.count()
```
`list_display` 改为`list_display = ("name", "hero_count", "villain_count")`

## 1.2 为ModelAdmin添加方法
如果你不想再model上添加方法， 你可以在ModelAdmin上添加
```Python
def hero_count(self, obj):
    return obj.hero_set.count()

def villain_count(self, obj):
    return obj.villain_set.count()
```
`list_display` 改为`list_display = ("name", "hero_count", "villain_count")`

## 1.3 计算字段，性能的考虑
上面2个方法，你将会进行为每个对象进行二次查询，
你可以查看[如何在Django Admin 优化查询]()

进行上述更改之后，你的admin就会像下面这样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/calculated_field.png)
