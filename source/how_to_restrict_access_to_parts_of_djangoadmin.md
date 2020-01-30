# 2.如何限定部分Django admin 权限
你可以使用权限系统启用和限制对Django admin特定部分的访问。 添加模型时，默认情况下，Django创建三个权限。 添加，更改和删除

管理员使用这些权限来决定用户的访问权限。 对于具有is_superuser = False且没有权限的用户，管理员看起来像这样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/access_no_perms.png
)
如果你添加一个权限` user.user_permissions.add(Permission.objects.get(codename="add_hero"))` 这admin页面会展示成为这样子：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/access_one_perm.png)
你可以通过更改以下方法来添加更复杂的逻辑来限制访问权限：
```Python
def has_add_permission(self, request):
    ...

def has_change_permission(self, request, obj=None):
    ...

def has_delete_permission(self, request, obj=None):
    ...

def has_module_permission(self, request):
    ...
```