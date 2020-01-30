# 3.如何让计算字段可以排序
Django在模型属性的字段上添加了排序功能。 当您添加计算字段时，Django不知道如何执行`order_by`，因此不会在该字段上添加排序功能。
如果你想让计算字段也可以排序，你需要告诉Django如何去`order_by` 你可以在计算字段方法上设置`admin_order_filed`属性。

从之前的章节[如何在django admin 中优化查询]()的页面中，添加如下：
```Python
hero_count.admin_order_field = '_hero_count'
villain_count.admin_order_field = '_villain_count'
```

修改了这些之后，你的admin就变成了这样：
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

    hero_count.admin_order_field = '_hero_count'
    villain_count.admin_order_field = '_villain_count'
```

![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/sorting_calculated_field.png)