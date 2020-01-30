# 2.如何在Django admin 优化查询
如果在你的admin有大量的计算字段，你可能会对每个对象进行多个查询，导致你的admin页面变得非常慢，要修复这个问题，你可以在
ModelAdmin上注释这些计算字段。
让我们以下面这个ModelAdmin 举例子，
```Python
@admin.register(Origin)
class OriginAdmin(admin.ModelAdmin):
    list_display = ("name", "hero_count", "villain_count")

    def hero_count(self, obj):
        return obj.hero_set.count()

    def villain_count(self, obj):
        return obj.villain_set.count()
``` 
这将为listview页面中的每一行添加两个额外的查询。要解决这个问题，你可以覆盖`get_queryset`来注释计算字段，然后在ModelAdmin方法中使用带注释的字段。

修改之后，你的ModelAdmin如下：
```Python
@admin.register(Origin)
class OriginAdmin(admin.ModelAdmin):
    list_display = ("name", "hero_count", "villain_count")

    def get_queryset(self, request):
        queryset = super().get_queryset(request)
        queryset = queryset.annotate(
            _hero_count=Count("hero", distinct=True),
            _villain_count=Count("villain", distinct=True),
        )
        return queryset

    def hero_count(self, obj):
        return obj._hero_count

    def villain_count(self, obj):
        return obj._villain_count
```
这样每个对象没有额外的查询。 你的管理页面像之前没有调用`annotate`那样
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/calculated_field.png)
