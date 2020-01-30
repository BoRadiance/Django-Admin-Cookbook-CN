# 1.如何限定为特定用户的使用Django admin
Django admin允许标志为 `is_staff=True`的用户访问，如果想让一个用户无法访问Django admin ，你应该设置`is_staff=False`

甚至这个用户是超级用户也是这样，non-staff的用户访问admin,将会看到这个：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/access_no_is_staff.png)
