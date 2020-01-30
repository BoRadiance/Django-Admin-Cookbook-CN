# 5.布尔的计算字段显示 ‘on’ 或者 ‘off’ 图标
在之前的章节 [如何让计算字段可过滤]()中，你添加了一个
布尔的字段 
```Python
def is_very_benevolent(self, obj):
    return obj.benevolence_factor > 75
```
就像这样:
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/filter_calculated_fixed.png)
这里的`is_very_benevolent`字段展示了 True 或者False，
不像内置的布尔字段展示on 或者 off 标识符， 为了解决这个问题
你可以在你的方法中添加一个boolean属性，你最后的modeladmin像这样：
```Python
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin):
    list_display = ("name", "is_immortal", "category", "origin", "is_very_benevolent")
    list_filter = ("is_immortal", "category", "origin", IsVeryBenevolentFilter)

    def is_very_benevolent(self, obj):
        return obj.benevolence_factor > 75

    is_very_benevolent.boolean = True
```
你的admin管理页面像这样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/boolean_field_fixed.png)



