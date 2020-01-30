# 3.Django admin 怎么移除删除action
Django Admin 默认是有*Delete Selected* action, 你被要求在`Hero`admin页面移除这个action

`ModelAdmin.get_actions`返回展示操作，通过重写方法移除`delete_selected` 。你的代码将会改成这个样子：
```Python
def get_actions(self, request):
    actions = super().get_actions(request)
    if 'delete_selected' in actions:
        del actions['delete_selected']
    return actions
```
你的admin页面像这样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/export_selected.png)

你应该也读一下[如何为一个model移除‘添加’/‘删除’按钮](https://django-admin-cookbook-cn.readthedocs.io/en/latest/how_to_remove_the_add_delete_button_for_a_model.html)

