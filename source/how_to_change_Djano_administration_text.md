# 1.怎么去修改‘Django administration’ 文字? 

Django admin 默认就是显示 'Django administration' . 你被要求使用‘UMSRA Administration’ 代替

文字在这些页面上：

- 登录页面
- 列表显示页面
- HTML的标题

## 1.1. 登录，列表，变更视图
默认情况下，它会像这样，并且设置成‘Django administration'
![](https://raw.githubusercontent.com/BoRadiance/Django-Admin-Cookbook-CN/master/source/img/2019-12-18_230055.png)

`site_header` 可以设置并修改它


## 1.2. 列表视图
默认情况下，它会像这样，并且设置成‘Site administration'
![](https://github.com/BoRadiance/Django-Admin-Cookbook-CN/blob/master/source/img/2019-12-18_230125.png)




`index_title` 可以设置并修改它

## 1.3.HTML标题标签
默认情况下，它会像这样，并且设置成‘Django site admin'
![](https://github.com/BoRadiance/Django-Admin-Cookbook-CN/blob/master/source/img/2019-12-18_230136.png)



`site_title` 可以设置并修改它

我们可以在url.py做出这3处修改：

```python
admin.site.site_header = "UMSRA Admin"
admin.site.site_title = "UMSRA Admin Portal"
admin.site.index_title = "Welcome to UMSRA Researcher Portal"
```

