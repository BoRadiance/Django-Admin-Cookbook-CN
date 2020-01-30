# 5.如何在创建时，字段可编辑，但是现有只读？
创建`Hero`对象后，你需要让`name`和`category`设置为只读，但是第一次写入的时候，这些字段需要可编辑

你可以重写`get_readonly_fields`方法，像这样：
```Python
def get_readonly_fields(self, request, obj=None):
    if obj:
        return ["name", "category"]
    else:
        return []
```
`obj`在对象创建期间是None，但是编辑期间，编辑对象期间是可编辑的。