# 9.如何在修改页面添加自定义的按钮
`Villain`有一个叫`is_unique`的字段
```Python
class Villain(Entity):
    ...
    is_unique = models.BooleanField(default=True)
```
你想在Villain页面添加一个叫“Make Unique”按钮让这个Villian唯一，其他的同名的villian都应该被删除。

你先扩展`change_form`添加一个按钮
```HTML
{% extends 'admin/change_form.html' %}

{% block submit_buttons_bottom %}
    {{ block.super }}
    <div class="submit-row">
            <input type="submit" value="Make Unique" name="_make-unique">
    </div>
{% endblock %}
```
然后你可以覆盖`response_change`并将模板连接到`VillainAdmin`

```Python
@admin.register(Villain)
class VillainAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    change_form_template = "entities/villain_changeform.html"


    def response_change(self, request, obj):
        if "_make-unique" in request.POST:
            matching_names_except_this = self.get_queryset(request).filter(name=obj.name).exclude(pk=obj.id)
            matching_names_except_this.delete()
            obj.is_unique = True
            obj.save()
            self.message_user(request, "This villain is now unique")
            return HttpResponseRedirect(".")
        return super().response_change(request, obj)
```
你现在的admin页面：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/custom_button.png)
