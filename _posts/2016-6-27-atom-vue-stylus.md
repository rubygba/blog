---
layout: post
title:  "Atom编辑器Vue插件设置"
date:   2016-04-17 14:30:00 +0800
categories: Atom Stylus plugins packages emmet
---

### 前言

在最新版中，Atom使用 `language-vue` 插件已经可以为 `.vue` 文件提供良好的代码高亮效果。但为了加快写代码的速度，代码补全是开发中不可缺少的功能，vue使用了嵌入的HTML、style和script文本，其中HTML的emmet插件默认无法调用、Stylus格式的style也无法正确调用stylus插件的补全功能。

### 为 `.vue` 文件添加emmet插件支持

Atom设置界面，打开：File -> Keymap，添加：

``` cson
'atom-text-editor[data-grammar="text html vue"]:not([mini])':
    'tab': 'emmet:expand-abbreviation-with-tab'
```

### 修改 `language-vue` 插件，使其正确识别Stylus

打开Atom配置文件夹 `用户名\.atom` ，插件文件夹 `用户名\.atom\packages\language-vue` ，配置文件 `language-vue\grammers\vue.cson`

原配置

``` cson
...

  {
    name: "source.stylus.embedded.html"
    begin: "(?:^\\s+)?(<)((?i:style))\\b(?=[^>]*lang=[\"']stylus[\"'])"
    end: "(</)((?i:style))(>)(?:\\s*\\n)?"
    captures:
      "1":
        name: "punctuation.definition.tag.html"
      "2":
        name: "entity.name.tag.style.html"
      "3":
        name: "punctuation.definition.tag.html"
    patterns: [
      {
        include: "#tag-stuff"
      }
      {
        begin: "(>)"
        beginCaptures:
          "1":
            name: "punctuation.definition.tag.html"
        end: "(?=</(?i:style))"
        patterns: [
          {
            include: "source.css.stylus"
          }
        ]
      }
    ]
  }

...
```

修改第一句就行
``` cson
...

  name: "source.css.stylus.embedded.html"

...
```
