---
title: JsonArray::copyFrom()
description: Fills a JsonArray
keywords: ArduinoJson,JsonArray,copyFrom
layout: api
tags: api
api-group: JsonArray
---

## Description

Populates the [`JsonArray`]({{site.baseurl}}/api/jsonarray/) with values from a C array.

## Signatures

```c++
JsonArray::copyFrom(int array[]);
JsonArray::copyFrom(double array[]);
JsonArray::copyFrom(const char* array[]);
```

## Return value

`true` if the operation is successful; or `false` if there was not enough room in the [`JsonBuffer`]({{site.baseurl}}/api/jsonbuffer/).

## Example

```c++
int values[] = {1, 2, 3};

StaticJsonBuffer<200> jsonBuffer;
JsonArray& array = jsonBuffer.createArray();
array.copyFrom(values);
array.printTo(Serial);
```

will write

```json
[1,2,3]
```

## See also

* [JsonArray::copyTo()]({{site.baseurl}}/api/jsonarray/copyto/)