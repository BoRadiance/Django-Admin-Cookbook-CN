# 2.如何添加一对一关系作为admin 内联
OneToOneFields 可以像外键一样设置为内联，但是，只能将OneToOneFields的一侧设置为inline模型

你有`HeroAcquaintance`模型，这个和hero有一对一关系，像这样：
```Python
class HeroAcquaintance(models.Model):
    "Non family contacts of a Hero"
    hero = models.OneToOneField(Hero, on_delete=models.CASCADE)
    ....
```
你可以添加作为内联，像这样：
```Python
class HeroAcquaintanceInline(admin.TabularInline):
    model = HeroAcquaintance


@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    inlines = [HeroAcquaintanceInline]
```

![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/one_to_one_inline.png)
