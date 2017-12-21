---
title: JsonObject::size()
description: Gets the number of element in a JsonObject
keywords: ArduinoJson,JsonObject,size,length
layout: api
tags: api
api-group: JsonObject
---

## Description

Returns the number of key/value pairs in the object.

## Signature

```c++
size_t size() const;
```

## Return value

An unsigned integer containing the number of key/value pairs in the object.

## Example

```c++
JsonObject& object = jsonBuffer.createObject();
object["hello"] = "world";
Serial.println(object.size()); // 1
```

## See also

* [`JsonArray::size()`]({{site.baseurl}}/api/jsonarray/size/)
