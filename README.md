# 介绍
## 译者的话
身为PythonWeb开发热爱者

希望能对开源社区做出一些贡献。

本人能力有限，难免有疏漏或者表意不当的地方。 如果译文中有什么错漏的地方请大家见谅，也欢迎大家随时指正。QQ: 189219902

[中文翻译](https://django-admin-cookbook-cn.readthedocs.io/en/latest/)

## Django Admin Cookbook介绍

Django Admin Cookbook 是一本关于Django Admin 模块的书

它面向Django的对DjangoAdmin有一些使用经验但是想要扩大对DjangoAdmin的认知甚至想要精通DjangoAdmin的中级Django开发者.

它采用问答的形式来讨论关于你可能会使用DjangoAdmin完成的常见任务。

所有章节均基于一组通用模型：

### App entities

```Python
class Category(models.Model):
    name = models.CharField(max_length=100)

    class Meta:
        verbose_name_plural = "Categories"

    def __str__(self):
        return self.name


class Origin(models.Model):
    name = models.CharField(max_length=100)

    def __str__(self):
        return self.name


class Entity(models.Model):
    GENDER_MALE = "Male"
    GENDER_FEMALE = "Female"
    GENDER_OTHERS = "Others/Unknown"

    name = models.CharField(max_length=100)
    alternative_name = models.CharField(
        max_length=100, null=True, blank=True
    )


    category = models.ForeignKey(Category, on_delete=models.CASCADE)
    origin = models.ForeignKey(Origin, on_delete=models.CASCADE)
    gender = models.CharField(
        max_length=100,
        choices=(
            (GENDER_MALE, GENDER_MALE),
            (GENDER_FEMALE, GENDER_FEMALE),
            (GENDER_OTHERS, GENDER_OTHERS),
        )
    )
    description = models.TextField()

    def __str__(self):
        return self.name

    class Meta:
        abstract = True


class Hero(Entity):

    class Meta:
        verbose_name_plural = "Heroes"

    is_immortal = models.BooleanField(default=True)

    benevolence_factor = models.PositiveSmallIntegerField(
        help_text="How benevolent this hero is?"
    )
    arbitrariness_factor = models.PositiveSmallIntegerField(
        help_text="How arbitrary this hero is?"
    )
    # relationships
    father = models.ForeignKey(
        "self", related_name="+", null=True, blank=True, on_delete=models.SET_NULL
    )
    mother = models.ForeignKey(
        "self", related_name="+", null=True, blank=True, on_delete=models.SET_NULL
    )
    spouse = models.ForeignKey(
        "self", related_name="+", null=True, blank=True, on_delete=models.SET_NULL
    )


class Villain(Entity):
    is_immortal = models.BooleanField(default=False)

    malevolence_factor = models.PositiveSmallIntegerField(
        help_text="How malevolent this villain is?"
    )
    power_factor = models.PositiveSmallIntegerField(
        help_text="How powerful this villain is?"
    )
    is_unique = models.BooleanField(default=True)
    count = models.PositiveSmallIntegerField(default=1)###
```

### App events

```python
    name = models.CharField(max_length=255)
    participating_heroes = models.ManyToManyField(Hero)
    participating_villains = models.ManyToManyField(Villain)


class Event(models.Model):
    epic = models.ForeignKey(Epic, on_delete=models.CASCADE)
    details = models.TextField()
    years_ago = models.PositiveIntegerField()


class EventHero(models.Model):
    event = models.ForeignKey(Event, on_delete=models.CASCADE)
    hero = models.ForeignKey(Hero, on_delete=models.CASCADE)
    is_primary = models.BooleanField()


class EventVillain(models.Model):
    event = models.ForeignKey(Event, on_delete=models.CASCADE)
    hero = models.ForeignKey(Villain, on_delete=models.CASCADE)
    is_primary = models.BooleanField()
```

## 如何使用这本书

你可以把这本书从头读到尾，也可以选择你需要的章节读，每一个章节是独立的，特定的任务

无论如何，你都应该先读`entities/models.py` 和 `events/models.py`



[英文原文Django Admin Cookbook](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/introduction.html)