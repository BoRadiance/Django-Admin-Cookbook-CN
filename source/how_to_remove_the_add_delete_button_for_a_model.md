# 4.如何使一个model移除'添加'/'删除'按钮
UMSRA管理层已添加所有`Category`和`Origin`对象，并希望禁用任何进一步的添加和删除。 他们要求你禁用“添加”和“删除”按钮。 您可以通过在Django管理员中重写`has_add_permission`和`has_delete_permission`来做到这一点：
```Python
def has_add_permission(self, request):
    return False

def has_delete_permission(self, request, obj=None):
    return False
```
通过这些修改，你的admin就像这样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/remove_add_perms.png)
注意移除*Add*按钮，这个添加和删除按钮也会从详情页面中删除，你还可以阅读[如何从Django Admin中移除删除action](https://django-admin-cookbook-cn.readthedocs.io/en/latest/how_to_remove_delete_selected_action.html)