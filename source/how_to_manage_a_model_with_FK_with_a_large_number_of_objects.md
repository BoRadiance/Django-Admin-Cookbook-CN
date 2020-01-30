# 7.如何管理一个有很多外键对象的模型？
你可以创建大量的category,像这样：
```Python
categories = [Category(**{"name": "cat-{}".format(i)}) for i in range(100000)]
Category.objects.bulk_create(categories)
```
现在`Category`超过了100000个对象，当你在`Hero`admin页面的时候，你将会有一个超过100000选择的下拉框。这会让页面变得很慢，并且下拉框难于使用。
你可以通过设置`raw_id_fields`来改变admin的管理方式。
```Python
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    raw_id_fields = ["category"]
```
修改了之后，Hero的admin页面像这样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/raw_id_fields_1.png)

弹出窗口像这样:
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/raw_id_fields_2.png)
