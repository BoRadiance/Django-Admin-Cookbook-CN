# 4.如何在Django-admin添加数据库视图
你有这样创建的数据库视图：
```sql
create view entities_entity as
    select id, name from entities_hero
    union
    select 10000+id as id, name from entities_villain
```
它具有`Hero`和`Villain`所有的名称，
Villain的id设置为10000+id,因为我们不打算横过10000名Heros
```sql
sqlite> select * from entities_entity;
1|Krishna
2|Vishnu
3|Achilles
4|Thor
5|Zeus
6|Athena
7|Apollo
10001|Ravana
10002|Fenrir
```
然后添加一个`managed=False`模型:
```Python
class AllEntity(models.Model):
    name = models.CharField(max_length=100)

    class Meta:
        managed = False
        db_table = "entities_entity"
```
并添加到admin:
```Python
@admin.register(AllEntity)
class AllEntiryAdmin(admin.ModelAdmin):
    list_display = ("id", "name")
```
你的admin看起来像这样：
![](https://books.agiliq.com/projects/django-admin-cookbook/en/latest/_images/database_view.png)
