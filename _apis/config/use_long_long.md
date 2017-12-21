---
title: ARDUINOJSON_USE_LONG_LONG
description: ARDUINOJSON_USE_LONG_LONG changes the type used to store integer values in [`JsonVariant`]({{site.baseurl}}/api/jsonvariant/).
keywords: ArduinoJson,long long,64
layout: api
tags: api
api-group: Configuration
---

Determines the type used to store integer values in [`JsonVariant`]({{site.baseurl}}/api/jsonvariant/).

* If `ARDUINOJSON_USE_LONG_LONG == 0`, then [`JsonVariant`]({{site.baseurl}}/api/jsonvariant/) stores a `long`
* If `ARDUINOJSON_USE_LONG_LONG == 1`, then [`JsonVariant`]({{site.baseurl}}/api/jsonvariant/) stores a `long long`

The default is `0` when `ARDUINO` is defined, `1` otherwise.
In other words, it's `long` on embedded systems, but `long long` on computers.

To override the default value, you need to `#define` the value before including `ArduinoJson.h`, like this:

```c++
#define ARDUINOJSON_USE_LONG_LONG 1 // we need to store long long
#include <ArduinoJson.h>
```

:warning: Caution: the memory consumption will be higher, and the results from the [ArduinoJson Assistant]({{site.baseurl}}/assistant/) will be wrong.

:warning: Caution: make sure you use the same value for each compilation unit, i.e., for each `.ino` and `.cpp` in your project.
