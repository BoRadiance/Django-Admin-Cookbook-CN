# 4.如何让计算字段可以过滤
你有`Hero`admin ，像下面这样：
```Python
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin):
    list_display = ("name", "is_immortal", "category", "origin", "is_very_benevolent")
    list_filter = ("is_immortal", "category", "origin",)

    def is_very_benevolent(self, obj):
        return obj.benevolence_factor > 75
```
如果有一个 `is_very_benevolent`计算字段，你的admin页面会像这样一样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/no_filtering.png)
你已经在模型字段中添加了过滤，但是你还想计算字段添加过滤，为了做到这个，你需要这样子类化SimpleListFilter

```Python
class IsVeryBenevolentFilter(admin.SimpleListFilter):
    title = 'is_very_benevolent'
    parameter_name = 'is_very_benevolent'

    def lookups(self, request, model_admin):
        return (
            ('Yes', 'Yes'),
            ('No', 'No'),
        )

    def queryset(self, request, queryset):
        value = self.value()
        if value == 'Yes':
            return queryset.filter(benevolence_factor__gt=75)
        elif value == 'No':
            return queryset.exclude(benevolence_factor__gt=75)
        return queryset
```
并且将你的list_filter改为：` list_filter = ("is_immortal", "category", "origin", IsVeryBenevolentFilter)`

这样你可以过滤你的计算字段，admin就变成了这样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/filter_calculated_fixed.png)
