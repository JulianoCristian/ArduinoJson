---
title: Why ArduinoJson is slow?
description: You can speed up ArduinoJson by using a buffer.
keywords: ArduinoJson,slow,speed,WifiClient,wifi,ESP8266
layout: faq
tags: faq
faq-group: Serialization
popularity: 114
---

First of all, ArduinoJson is **not slow** by itself. It's slow when used in conjunction with the `WifiClient` from the ESP8266 core.

The problem is that there is no buffer between ArduinoJson and the WifiClient.

To solve this, either:

1. Enable the [Nagle algorithm](https://en.wikipedia.org/wiki/Nagle%27s_algorithm) on `WifiClient` by calling `setNoDelay(false)`.
2. Serialize to a buffer and send the whole buffer in one shot.
3. Insert a [BufferedPrint]({{ site.baseurl }}/doc/tricks/#buffered-output) proxy between ArduinoJson and `WifiClient`.
