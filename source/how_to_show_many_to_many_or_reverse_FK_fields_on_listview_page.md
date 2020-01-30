# 4.如何在列表视图页面显示多对多或者反转FK字段
对于英雄，你可以使用以下字段跟踪到它的父亲
```Python
father = models.ForeignKey(
    "self", related_name="children", null=True, blank=True, on_delete=models.SET_NULL
)
```
你被要求在列表页面上显示每个英雄的孩子，Hero 对象有`children` 反转FK属性，
但是你不能将它添加到`list_display`里，你需要向ModelAdmin添加一个属性，并在list_display中使用它，你可以这样做：
```Python
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...

    def children_display(self, obj):
        return ", ".join([
            child.name for child in obj.children.all()
        ])
    children_display.short_description = "Children"
```
你可以看到children列，想这样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/related_fk_display.png)
对于多对多关系，你可以使用相同的方法。 你应该也阅读一下[如何获取特定对象的admin url]()