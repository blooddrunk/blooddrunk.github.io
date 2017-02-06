---
title: 让Redcarpet在Jekyll Github Pages中处理表格
date: 2015-03-24 23:15:10
tags:
  - Jekyll
  - blog
---

在`_config.yml`指定markdown解析器为[Redcarpet](https://github.com/vmg/redcarpet)后，还需[配置](http://jekyllrb.com/docs/configuration/#redcarpet)其`extensions`来正确处理表格，值的格式为字符串数组

{% codeblock lang:yaml %}
# in _config.yml
markdown: redcarpet
redcarpet:
  extensions: ["tables"]
{% endcodeblock %}

<!-- more -->

这样就可以用以下格式来书写表格，`:`冒号的位置代表了对齐的方向：


```
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

```

输出：


| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |


其他可选的`extensions`参考Redcarpet的[README](https://github.com/vmg/redcarpet/blob/v2.2.2/README.markdown#and-its-like-really-simple-to-use)

{% codeblock lang:yaml %}
# in _config.yml
markdown: redcarpet
redcarpet:
  extensions: ["no_intra_emphasis", "fenced_code_blocks", "autolink", "tables", "with_toc_data"]
{% endcodeblock %}