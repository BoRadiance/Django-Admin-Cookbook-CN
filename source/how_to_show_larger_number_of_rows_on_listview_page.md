# 1./2.如何列表视图上显示大量行/不分页
你被要求在单个页面上看到英雄增加到250（默认100），你可以这样做：
```Python
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    list_per_page = 250
```
你也可以将其设置为较小的值，如果将它设置为1 `list_per_page=1` ，admin页面就像这样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/icrease_row_count_to_one.png)

如何使得不分页呢？ 
```Python
import sys
...

@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...

    list_per_page = sys.maxsize
```
