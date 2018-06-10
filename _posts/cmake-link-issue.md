---
title: CMake link issue
date: 2017-09-27 15:15:27
tags:
- cmake
---

When we use cmake to compile our program and use `TARGET_LINK_LIBRARIES` to link libraries we need, we have to pay attention to the order these libraries are specified.

In the following case, our program depends on the muduo_base library and muduo_base library depends on pthread, then we have to ensure pthread is after muduo_base:
{% codeblock lang:cmake %}
TARGET_LINK_LIBRARIES(${PROJECT_NAME} ${MUDUO_NET_LIBRARY} ${MUDUO_BASE_LIBRARY} pthread)
{% endcodeblock %}
Otherwise, we would get a link error if we specify like this:
{% codeblock lang:cmake %}
TARGET_LINK_LIBRARIES(${PROJECT_NAME} pthread ${MUDUO_NET_LIBRARY} ${MUDUO_BASE_LIBRARY})
{% endcodeblock %}

