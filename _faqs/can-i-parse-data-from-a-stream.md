---
title: Can I parse data from a stream?
description: It's possible to parse JSON from a Stream
keywords: ArduinoJson,Stream
layout: faq
tags: faq
faq-group: Deserialization
popularity: 82
---

Yes.

Since ArduinoJson 5.8, `parseArray()` and `parseObject()` accept Arduino's `Stream` and `std::istream` as input:

```c++
JsonObject& root = jsonObject.parseObject(myStream);
```

Parts of the input need to be copied into the [`JsonBuffer`]({{site.baseurl}}/api/jsonbuffer/), so you need to increase its capacity accordingly (the [Assistant]({{site.baseurl}}/assistant/) gives the required size).

The parser only copies the relevant part of the input, skipping the spaces and the punctuation.
This is way more efficient than copying the whole input in a `char[]` and then call `parseObject()`.

> ## Example: parse JSON from SPIFFS
>
> ```c++
> // Initialize SPIFFS
> SPIFFS.begin();
>
> // Open the File (which implements Stream)
> File file = SPIFFS.open("config.json", "r");
>
> // Let ArduinoJson read directly from File
> DynamicJsonBuffer jb;
> JsonObject& config = jb.parseObject(file);
>
> // We don't need the file anymore
> file.close()
> ```
{:.alert .alert-success}

See:

* [Examples: JsonHttpClient.ino]({{site.baseurl}}/example/http-client/)
* [API Reference: JsonBuffer::parseArray()]({{site.baseurl}}/api/jsonbuffer/parsearray/)
* [API Reference: JsonBuffer::parseObject()]({{site.baseurl}}/api/jsonbuffer/parseobject/)

