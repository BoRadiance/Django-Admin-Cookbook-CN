# 5.怎么给Django-admin添加图标

UMSRA 的领导现喜欢你到目前创建的admin，但是市场营销人员希望你在所有的admin页面添加UMSRA logo图标

你需要在django settings重写django提供的默认templates 
django设置*TEMPLATES*的代码像这样：

```Python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```
这意味着Django将在每个应用程序内的名为`template`的目录中查找模板，
但是你可以通过重写TEMPLATES.DIRS 的值，
我可以把`'DIRS': []`转换成` 'DIRS': [os.path.join(BASE_DIR, 'templates/')],`
并且创建一个`templaes`文件夹。 如果你的`STATICFILE_DIRS`是空的话，就设置：
```Python
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, "static"),
]
```
现在从admin app 里面拷贝`base_site.html`到你刚刚创建的`templates\admin`文件夹下，替换默认的品牌标记块
```HTML
<h1 id="site-name">
    <a href="{% url 'admin:index' %}">
        <img src="{% static 'umsra_logo.png' %}" height="40px" />
    </a>
</h1>
```
改变你的base_site.html之后，将会像这样：
```HTML
{% extends "admin/base.html" %}

{% load staticfiles %}

{% block title %}{{ title }} | {{ site_title|default:_('Django site admin') }}{% endblock %}

{% block branding %}
<h1 id="site-name">
    <a href="{% url 'admin:index' %}">
        <img src="{% static 'umsra_logo.png' %}" height="40px" />
    </a>
</h1>
{% endblock %}

{% block nav-global %}{% endblock %}
```
你的admin会像这样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/logo_fixed.png)