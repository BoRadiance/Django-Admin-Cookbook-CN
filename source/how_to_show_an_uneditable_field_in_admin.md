# 4.如何显示不可编辑的字段
如果你的模型有一个`editable=False` 字段，默认情况下，这个字段会隐藏在更改页面中，对应标记为`auto_now`或`auto_now_add`的任何字段也是一样的。因为这些字段上会设置`editable=False`
如果你希望这些字段显示在更改页面上，可以将他们添加到只读字段。
```Python
@admin.register(Villain)
class VillainAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    readonly_fields = ["added_on"]
```
进行这些修改，Villain admin页面看起来会像这样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/uneditable_existing.png)
