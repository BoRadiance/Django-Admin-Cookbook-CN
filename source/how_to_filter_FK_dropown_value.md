# 6.如何在Django-admin 过滤 外键下拉
`Hero` 模型类有 `Category` 外键。
所以，所有的category对象将出现在管理页面下拉列表中。
另外，你如果只想查看一个子集，则Django允许你通过覆盖formfield_for_foreignkey对其进行自定义：
```Python
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    def formfield_for_foreignkey(self, db_field, request, **kwargs):
        if db_field.name == "category":
            kwargs["queryset"] = Category.objects.filter(name__in=['God', 'Demi God'])
        return super().formfield_for_foreignkey(db_field, request, **kwargs)
```
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/filter_fk_dropdown.png)
