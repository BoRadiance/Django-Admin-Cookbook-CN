# 1.如何在Django admin添加额外的actions
Django admin 可以允许你添加额外的actions，这些actions可以允许你进行批量操作，你现在被要求添加一个将多个`Hero`标记为不朽的action
你可以通过ModelAdmin加上该方法，并将该方法作为字符串添加到`actions`中
```Python
actions = ["mark_immortal"]

def mark_immortal(self, request, queryset):
    queryset.update(is_immortal=True)

```