---
title: JsonBuffer::parseArray()
description: Parses a JSON array
keywords: ArduinoJson,parse,JSON,array,JsonArray
layout: api
tags: api
api-group: JsonBuffer
---

## Description

Allocates and populate a [`JsonArray`]({{site.baseurl}}/api/jsonarray/) from a JSON string.

By design, the parser needs to alter the string to insert null-terminators and replace escaped chars.
If the JSON string is read-only, it will have to duplicate the input string, this consume more space in the [`JsonBuffer`]({{site.baseurl}}/api/jsonbuffer/).
Therefore, it's recommended to have a JSON input in a `char[]` or a `char*`.

## Signatures

```c++
// The first overload, which accepts a modifiable array of chars, is the most efficient
// since it allows the zero-copy feature.
JsonArray& parseArray(char* json, uint8_t nestingLimit=10);

// The following overloads, which accept read-only strings, require a bigger JsonBuffer
// because parts of the JSON input has to be copied.
JsonArray& parseArray(const char* json, uint8_t nestingLimit=10);
JsonArray& parseArray(const String& json, uint8_t nestingLimit=10);
JsonArray& parseArray(const std::string& json, uint8_t nestingLimit=10);
JsonArray& parseArray(const __FlashStringHelper* json, uint8_t nestingLimit=10);

// The two last overloads, which accept input streams, make copy of the input too.
JsonArray& parseArray(Stream& json, uint8_t nestingLimit=10);
JsonArray& parseArray(std::istream& json, uint8_t nestingLimit=10);
```

## Arguments

`json` is the input string to be parsed.

`nestingLimit` specifies the maximum level of nesting allowed in the JSON string.
If set to `0`, only a flat array can be parsed.
If set to `1`, the array can contain nested arrays or objects but only 1 level deep.
And bigger values will allow more level of nesting.
The purpose of this feature is to prevent stack overflow that could lead to a security risk.

## Return value

Returns a reference to the new [`JsonArray`]({{site.baseurl}}/api/jsonarray/) or `JsonArray::invalid() if the allocation fails.

## Example

```c++
char json[] = "[\"hello\",\"world\"]";
StaticJsonBuffer<200> jsonBuffer;
JsonArray& array = jsonBuffer.parseArray(json);
const char* hello = array[0];
const char* world = array[1];
```

## See also

* [`JsonBuffer::parseObject()`]({{site.baseurl}}/api/jsonbuffer/parseobject/)
* [`JsonBuffer::parse()`]({{site.baseurl}}/api/jsonbuffer/parse/)