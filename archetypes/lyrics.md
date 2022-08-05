---
title: 
subtitle: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: false
categories: ["lyrics"]
twemoji: true
---

# {{ .Name }}
## 誰に聴いてほしいか

## 歌詞
{{% typeit code=text %}}
Here lyrics...
{{% /typeit %}}