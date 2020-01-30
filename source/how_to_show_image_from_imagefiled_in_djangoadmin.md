# 1.如何在Django admin 显示Imagefield 的图片
在你的`Hero`模型，你有一个图片字段
```Python
headshot = models.ImageField(null=True, blank=True, upload_to="hero_headshots/")
```
默认是这样显示的：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/imagefield.png)
你被要求将其更改的实际图片也显示在页面上，你可以这样做：
```Python
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):

    readonly_fields = [..., "headshot_image"]

    def headshot_image(self, obj):
        return mark_safe('<img src="{url}" width="{width}" height={height} />'.format(
            url = obj.headshot.url,
            width=obj.headshot.width,
            height=obj.headshot.height,
            )
    )
```
通过这些修改之后，你的imagefiled像这个样子：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/imagefield_fixed.png)
