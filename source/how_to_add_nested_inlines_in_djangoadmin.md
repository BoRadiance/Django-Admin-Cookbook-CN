# 3.如何在Django admin 中添加嵌套内联
你定义了下面这样的模型:
```Python
class Category(models.Model):
    ...

class Hero(models.Model):
    category = models.ForeignKey(Catgeory)
    ...

class HeroAcquaintance(models.Model):
    hero = models.OneToOneField(Hero, on_delete=models.CASCADE)
    ...
```
你想要一个admin页面创建`Category`，`Hero`和`HeroAcquaintance`对象。但是，Django不支持嵌套在多个级别上的外键内联或一对一关系。你有几个选择。
你可以修改`HeroAcquaintance`模型。使其具有直接的FK到`Category`，如下所示：
```Python
class HeroAcquaintance(models.Model):
    hero = models.OneToOneField(Hero, on_delete=models.CASCADE)
    category = models.ForeignKey(Category)

    def save(self, *args, **kwargs):
        self.category = self.hero.category
        super().save(*args, **kwargs)
```
然后，你可以将HeroAcquaintanceInline附加到CategoryAdmin，并获得一种嵌套内联。

另外，也有一些第三方Django应用允许嵌套内联。 Github或DjangoPackages搜索将找到适合你需求和品味的应用。