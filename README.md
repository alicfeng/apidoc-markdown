# apidoc_markdown
基于 `apidoc` 工具生成的接口文档转 `markdown` 文档

## 前言
使用 `apidoc` 工具生成接口文档，其实已经满足接口文档查阅所需，然而接口文档被按照统一格式维护与管理(对外-markdown)，为满足😌两者共存，于是就出现了 `apidoc-markdown` 😁其实已经有一个 `apidoc-markdown` 这样的扩展 `npm install -g apidoc-markdown`,只是不满足所需，刚需 - 结合 `docsify` 而用。

## 安装
```powershell
composer require alicfeng/apidoc_markdown --dev -vvv
```
## 使用
#### 帮助使用
```shell
➜ vendor/bin/apidoc_markdown.php
Usage: apidoc_markdown [options...]
-i    input directory  
-o    output directory 
```

#### 导出使用
```shell
➜ vendor/bin/apidoc_markdown.php -i {input} -o {output}
```
