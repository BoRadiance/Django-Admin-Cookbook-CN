# 2.如何从djagno admin 导出CSV
你被要求可以从admin导出`Hero` 和 `VillanAdmin`，有很多第三方模块可以做到。但是不依赖其他项也非常容易。你将添加admin action 到 `HeroAdmin` 和 `VillanAdmin`。

一个admin action可以总是有这样的函数签名（声明） `def admin_action(modeladmin, request, queryset):`或者你可以直接在ModelAdmin中作为一个方法。

```Python
class SomeModelAdmin(admin.ModelAdmin):

    def admin_action(self, request, queryset):
```

为了`HeroAdmin`添加csv导出，你可以像下面这样做：
```Python
actions = ["export_as_csv"]

def export_as_csv(self, request, queryset):
    pass

export_as_csv.short_description = "Export Selected"
```
添加了叫export selected 的action, 就像这样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/export_selected.png)
你将修改`export_as_csv` 如下：
```Python
import csv
from django.http import HttpResponse
...

def export_as_csv(self, request, queryset):

    meta = self.model._meta
    field_names = [field.name for field in meta.fields]

    response = HttpResponse(content_type='text/csv')
    response['Content-Disposition'] = 'attachment; filename={}.csv'.format(meta)
    writer = csv.writer(response)

    writer.writerow(field_names)
    for obj in queryset:
        row = writer.writerow([getattr(obj, field) for field in field_names])

    return response
```
这将导出所有选中行，如果你注意到，`export_as_csv` 没有`Hero`的内容，你可以将方法提取成一个mixin类。

进行这些修改之后，你的代码会变成这样：
```Python
class ExportCsvMixin:
    def export_as_csv(self, request, queryset):

        meta = self.model._meta
        field_names = [field.name for field in meta.fields]

        response = HttpResponse(content_type='text/csv')
        response['Content-Disposition'] = 'attachment; filename={}.csv'.format(meta)
        writer = csv.writer(response)

        writer.writerow(field_names)
        for obj in queryset:
            row = writer.writerow([getattr(obj, field) for field in field_names])

        return response

    export_as_csv.short_description = "Export Selected"


@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    list_display = ("name", "is_immortal", "category", "origin", "is_very_benevolent")
    list_filter = ("is_immortal", "category", "origin", IsVeryBenevolentFilter)
    actions = ["export_as_csv"]

...


@admin.register(Villain)
class VillainAdmin(admin.ModelAdmin, ExportCsvMixin):
    list_display = ("name", "category", "origin")
    actions = ["export_as_csv"]
```
继承`ExportCsvMixin`类其他的models同样可以导出csv