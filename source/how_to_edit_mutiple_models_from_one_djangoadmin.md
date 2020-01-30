# 1.如何在一个Django admin 页面编辑多个模型
为了达到这个目标，你需要使用内联
你有`Category`模型，你需要在Category管理页面添加、编辑`Villain`模型
```Python
class VillainInline(admin.StackedInline):
    model = Villain

@admin.register(Category)
class CategoryAdmin(admin.ModelAdmin):
    ...

    inlines = [VillainInline]
```
你可以看到在`Category`页面有添加/编辑`Villain`，如果内联的模型有很多字段，使用`StackedInline`，也可以使用`TabularInile`。
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/edit_multiple_models.png)