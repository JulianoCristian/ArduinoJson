---
title: How to prevent string escaping?
description: To present string escaping in JSON, wrap with type RawJson
keywords: ArduinoJson,escape,string,backslash
layout: faq
tags: faq
faq-group: Serialization
popularity: 36
---

String escaping is the feature that allows to have special characters in your encoded string.

For example:

```c++
DynamicJsonBuffer jsonBuffer;
JsonObject& root = jsonBuffer.createObject();
root["hello"] = "[1,2,3]";
root.printTo(Serial);
```

would print:

```json
{"hello":"[1,2,3]"}
```

If you want to disable string escaping, you need to wrap the `char*` with a `RawJson`, like this:

```c++
DynamicJsonBuffer jsonBuffer;
JsonObject& root = jsonBuffer.createObject();
root["hello"] = RawJson("[1,2,3]");
root.printTo(Serial);
```

which would print:

```json
{"hello":[1,2,3]}
```

**CAUTION**: There is one caveat! Unlike `const char*`, `std::string` and `String`, the `RawJson` string is not duplicated, so you need to make sure it's still in memory when you call [`printTo()`]({{site.baseurl}}/api/jsonobject/printto/).
You can still duplicate manually by calling [`JsonBuffer::strdup()`]({{site.baseurl}}/api/jsonbuffer/strdup/), like this:

```c++
DynamicJsonBuffer jsonBuffer;
JsonObject& root = jsonBuffer.createObject();
root["hello"] = RawJson("[1,2,3]");
{
    String s = "[1,2,3]";
    root["hello"] = RawJson(jsonBuffer.strdup(s));
    // s is destructed here, but that's ok because we made a copy
}
root.printTo(Serial);
```
