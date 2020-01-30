# 1.如何获取特殊对象的django admin url
你有显示每个英雄孩子的孩子列，你被要求将每个孩子的链接放到更改页面，你可以这样做：
```Python
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...

    def children_display(self, obj):
        display_text = ", ".join([
            "<a href={}>{}</a>".format(
                    reverse('admin:{}_{}_change'.format(obj._meta.app_label, obj._meta.model_name),
                    args=(child.pk,)),
                child.name)
             for child in obj.children.all()
        ])
        if display_text:
            return mark_safe(display_text)
        return "-"
```
`reverse('admin:{}_{}_change'.format(obj._meta.app_label, obj._meta.model_name), args=(child.pk,))` 会给出对象的url

其他选项：
删除：

` reverse('admin:{}_{}_delete'.format(obj._meta.app_label, obj._meta.model_name), args=(child.pk,))`

历史记录：
` reverse('admin:{}_{}_history'.format(obj._meta.app_label, obj._meta.model_name), args=(child.pk,))`

