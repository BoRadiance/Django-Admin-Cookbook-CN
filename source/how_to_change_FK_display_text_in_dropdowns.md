# 8.如何在下拉菜单中更改外键显示的文本
`Hero`有`Category`外键，在下拉框里，显示的只是名字，但是你想显示"Category:name"格式。
你可以在`Category`修改`__str__`方法，但是你只希望在admin中进行更改，你可以通过创建一个`forms.ModelChoiceField`的子类，并且自定义`label_from_instance`方法。
```Python
class CategoryChoiceField(forms.ModelChoiceField):
     def label_from_instance(self, obj):
         return "Category: {}".format(obj.name)
```
之后我们可以重写category的以用到这个字段`formfield_for_foreignkey`
```Python
def formfield_for_foreignkey(self, db_field, request, **kwargs):
    if db_field.name == 'category':
        return CategoryChoiceField(queryset=Category.objects.all())
    return super().formfield_for_foreignkey(db_field, request, **kwargs)
```
你的admin看起来会想这样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/fk_display.png)

