# 5. 如何使用Django admin 上传CSV 
你被要求在`Hero` admin页面可以上传csv，你需要添加一个链接到`Hero`更改列表页面，该页面将转到带有上传表单的页面。 你将为POST操作编写一个处理程序，以从创建csv对象：
```Python
class CsvImportForm(forms.Form):
    csv_file = forms.FileField()

@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    change_list_template = "entities/heroes_changelist.html"

    def get_urls(self):
        urls = super().get_urls()
        my_urls = [
            ...
            path('import-csv/', self.import_csv),
        ]
        return my_urls + urls

    def import_csv(self, request):
        if request.method == "POST":
            csv_file = request.FILES["csv_file"]
            reader = csv.reader(csv_file)
            # Create Hero objects from passed in data
            # ...
            self.message_user(request, "Your csv file has been imported")
            return redirect("..")
        form = CsvImportForm()
        payload = {"form": form}
        return render(
            request, "admin/csv_form.html", payload
        )
```
然后通过重写`admin/change_list.html`模板创建`entites/heroes_changelist.html`模块：
```HTML
{% extends 'admin/change_list.html' %}

{% block object-tools %}
    <a href="import-csv/">Import CSV</a>
    <br />
    {{ block.super }}
{% endblock %}

```
最后创建`csv_form.html`，像这样:
```HTML
{% extends 'admin/base.html' %}

{% block content %}
    <div>
        <form action="." method="POST" enctype="multipart/form-data">
            {{ form.as_p }}
            {% csrf_token %}

                <button type="submit">Upload CSV</button>
        </form>
    </div>
    <br />

{% endblock %}
```

通过这些更改之后，你会在`Hero`修改列表页获得一个链接
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/import_1.png)
这个是上传csv页面像这样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/import_2.png)
