---
title: JsonObject::createNestedArray()
description: Creates a JsonArray in a JsonObject
keywords: ArduinoJson,JsonObject,createNestedArray
layout: api
tags: api
api-group: JsonObject
---

## Description

Creates a `JsonArray` as a child of the current object.

## Signatures

```c++
JsonArray& createNestedArray(const char* key) const;
JsonArray& createNestedArray(const String& key) const; // duplicates key
JsonArray& createNestedArray(const std::string& key) const; // duplicates key
JsonArray& createNestedArray(const __FlashStringHelper* key) const; // duplicates key
```

## Arguments

`key`: the key of the array in the object, can be a `const char*` or a `const String&`

## Remarks

When you add a value using a `String` for key, a copy of the string is made, causing the [`JsonBuffer`]({{site.baseurl}}/api/jsonbuffer/) to grow.
The memory allocated for the copy will only be freed when the whole [`JsonBuffer`]({{site.baseurl}}/api/jsonbuffer/) is discarded.
To avoid this behavior, use a `const char*` key instead.

## Return value

A reference to the new `JsonArray`.
You can check `JsonArray::success()` to verify that the allocation succeeded.

## Example

```c++
StaticJsonBuffer<256> jsonBuffer;
JsonObject& root = jsonBuffer.createObject();
root["status"] = "on";
JsonArray& levels = root.createNestedArray("levels");
levels.add(10);
levels.add(30);
root.prettyPrintTo(Serial);
```

will print

```json
{
  "status": "on",
  "levels": [
    10,
    20
  ]
}
```

## See also

* [`JsonObject::createNestedObject()`]({{site.baseurl}}/api/jsonobject/createnestedobject/)
* [`JsonArray::createNestedArray()`]({{site.baseurl}}/api/jsonarray/createnestedarray/)