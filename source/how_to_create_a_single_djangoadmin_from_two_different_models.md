# 4.如何从2个不同的模型创建单个Django admin
`Hero`有外键`Category`，所以你可以在Hero的管理页面选择category,如果你还想从Hero管理页面创建Category对象，你可以修改Hero admin的表单，并自定义`save_model`行为：
```Python
class HeroForm(forms.ModelForm):
    category_name = forms.CharField()

    class Meta:
        model = Hero
        exclude = ["category"]


@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    form = HeroForm
    ....

    def save_model(self, request, obj, form, change):
        category_name = form.cleaned_data["category_name"]
        category, _ = Category.objects.get_or_create(name=category_name)
        obj.category = category
        super().save_model(request, obj, form, change)
```
进行修改后，你的管理页面所示，允许从Hero管理页面创建或更新`Category`。
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/single_admin_multiple_model.png)

