# 3.怎么去创建2个独立的admin站点

创建admin页面的通常方法是把所有的模型放到单一的admin文件中，然而，在单一的Django 应中，我们可以有多个admin站点
现在我们的`entity` 和 `event` 模型在同一个位置，UMSRA有2个不同的组去研究*Events* 和  *Entities*，所以希望将对这个2个模型的管理分开

我们将保持对*entities*的默认的admin保持不变，并且为*events*创建新的`AdminSite` 
在 我们的 events/admin.py:

```Python
from django.contrib.admin import AdminSite
class EventAdminSite(AdminSite):
    site_header = "UMSRA Events Admin"
    site_title = "UMSRA Events Admin Portal"
    index_title = "Welcome to UMSRA Researcher Events Portal"

event_admin_site = EventAdminSite(name='event_admin')


event_admin_site.register(Epic)
event_admin_site.register(Event)
event_admin_site.register(EventHero)
event_admin_site.register(EventVillain)
```
并改变url.py：
```Python
from events.admin import event_admin_site

urlpatterns = [
    path('entity-admin/', admin.site.urls),
    path('event-admin/', event_admin_site.urls),
]
```
这样就实现了分隔admin,2个管理都在各自的url下可以使用 `/entity-admin/` 和 	`event-admin/`