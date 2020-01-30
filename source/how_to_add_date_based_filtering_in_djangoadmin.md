# 3.Django admin 如何添加基于日期的过滤
你可以通过设置`date_hierarchy`在任何日期字段添加基于日期的过滤：
```Python
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    date_hierarchy = 'added_on'
```
看起来像这样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/date_filtering.png)

对于很多对象，这会非常消耗性能，另外，你可以继承SimpleListFilter的子类，并仅允许过滤年份或月份。