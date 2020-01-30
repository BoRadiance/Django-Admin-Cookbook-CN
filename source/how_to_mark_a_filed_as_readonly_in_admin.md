# 3.如何标记一个字段只读？ 
UMSRA临时决定停止追踪神话人物的家谱。 你被要求将`father`，`mother`和`spouse`字段设为只读。
你可以这样做：
```Python
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    readonly_fields = ["father", "mother", "spouse"]
```
