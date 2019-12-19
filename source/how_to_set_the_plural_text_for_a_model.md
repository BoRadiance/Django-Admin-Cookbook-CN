# 怎么为一个模型设置复数文字
默认情况下，admin给你的模型末尾加上‘s’ ,  又称做你模型的复数形式，像这个样子：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/plural.png)

你被要求设置正常的复数形式，*Categories and Heroes*

你可以通过在模型里面设置 verbose_name_plural 来达到这个目标，在你的模型中修改：

```python
class Category(models.Model):
    ...
    
    class Meta:
        verbose_name_plural = "Categories"

        
class Hero(Entity):
    ...
    
    class Meta:
        verbose_name_plural = "Heroes"
```
修改之后，你的admin就会成为这样子：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/plural_fixed.png)

