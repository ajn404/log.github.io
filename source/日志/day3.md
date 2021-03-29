---
title:今天看了以下之前写的代码，发现不少问题
---

### 关于setstate

每次setstate都会重新渲染，而我有个组件的usestate长达十几行，如何能更高效的管理state呢？