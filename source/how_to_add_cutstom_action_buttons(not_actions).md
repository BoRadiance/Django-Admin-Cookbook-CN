# 4.如何在Django Admin 列表页添加自定义操作按钮（不是actions）
UMSRA 决定只要有足够的kryptonite，所有英雄都是不朽的，然而，你想要改变他们的想法并说所有的英雄都是不朽的。

你被要求添加2个按钮，一个按钮可以使得所有英雄变得平凡，
另一个按钮可以使得所有英雄变得不朽。 因为它影响所有的英雄而与选择无关，所以需要一个单独的按钮，而不是action下拉框。

实现，我们需要修改`HeroAdmin`的模板以至于我们可以添加2个按钮
```Python
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    change_list_template = "entities/heroes_changelist.html"
```
然后我们重写`get_urls`， 并且在这个model admin 添加`set_immortal`和`set_mortal` 方法,他们将作为2个视图方法

```Python
def get_urls(self):
    urls = super().get_urls()
    my_urls = [
        path('immortal/', self.set_immortal),
        path('mortal/', self.set_mortal),
    ]
    return my_urls + urls

def set_immortal(self, request):
    self.model.objects.all().update(is_immortal=True)
    self.message_user(request, "All heroes are now immortal")
    return HttpResponseRedirect("../")

def set_mortal(self, request):
    self.model.objects.all().update(is_immortal=False)
    self.message_user(request, "All heroes are now mortal")
    return HttpResponseRedirect("../")
```
最后，我们创建继承`admin/change_list.html`的`entities/heroes_changelist.html`模板。

```Python
{% extends 'admin/change_list.html' %}

{% block object-tools %}
    <div>
        <form action="immortal/" method="POST">
            {% csrf_token %}
                <button type="submit">Make Immortal</button>
        </form>
        <form action="mortal/" method="POST">
            {% csrf_token %}
                <button type="submit">Make Mortal</button>
        </form>
    </div>
    <br />
    {{ block.super }}
{% endblock %}
```
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/action_buttons.png)
当使用*make_mortal* action之后，所有的英雄将会变得平凡，如你看到这个消息
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/action_button_message.png)
