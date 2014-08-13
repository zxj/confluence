---
layout: default
title: ECMA5中定义的Object
---

###non-extensible

不能添加新的属性，但可以修改已有的属性值，且可以删除属性

###non-configurable（sealed）

不能添加删除属性，可以修改已有的属性值。

###frozen

non-extensible、non-configurable、non-enumrable、不能修改属性值