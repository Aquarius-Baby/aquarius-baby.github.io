---
title: hexo使用渲染公式
date: 2020-02-02 00:23:40

categories:
    - 教程
tags:
    - hexo
---

hexo渲染公式，修改配置文件
https://blog.csdn.net/littlehaes/article/details/84370393

### 安装Kramed
```
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-kramed --save
```
### 更改文件配置
文件：/node_modules/hexo-renderer-kramed/lib/renderer.js
```
function formatText(text) {
    // Fit kramed's rule: $$ + \1 + $$
    return text.replace(/`\$(.*?)\$`/g, '$$$$$1$$$$');
}
```
修改为
```
// Change inline math rule
function formatText(text) {
    return text;
}
```
### 停用hexo-math并安装mathjax
```
npm uninstall hexo-math --save
npm install hexo-renderer-mathjax --save
```

### 更新Mathjax配置文件
文件：/node_modules/hexo-renderer-mathjax/mathjax.html

最后一行修改为
```
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>
```
### 更改默认转义规则
因为LaTeX与markdown语法有语义冲突，所以 hexo 默认的转义规则会将一些字符进行转义，所以我们需要对默认的规则进行修改.
打开/node_modules\kramed\lib\rules\inline.js
```
escape: /^\\([`*\[\]()#$+\-.!_>])/,
em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```
修改后：
```var inline = {
  // escape: /^\\([\\`*{}\[\]()#$+\-.!_>])/,
  escape: /^\\([`*\[\]()#$+\-.!_>])/,
  autolink: /^<([^ >]+(@|:\/)[^ >]+)>/,
  url: noop,
  html: /^<!--[\s\S]*?-->|^<(\w+(?!:\/|[^\w\s@]*@)\b)*?(?:"[^"]*"|'[^']*'|[^'">])*?>([\s\S]*?)?<\/\1>|^<(\w+(?!:\/|[^\w\s@]*@)\b)(?:"[^"]*"|'[^']*'|[^'">])*?>/,
  link: /^!?\[(inside)\]\(href\)/,
  reflink: /^!?\[(inside)\]\s*\[([^\]]*)\]/,
  nolink: /^!?\[((?:\[[^\]]*\]|[^\[\]])*)\]/,
  reffn: /^!?\[\^(inside)\]/,
  strong: /^__([\s\S]+?)__(?!_)|^\*\*([\s\S]+?)\*\*(?!\*)/,
  // em: /^\b_((?:__|[\s\S])+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
  em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
  code: /^(`+)\s*([\s\S]*?[^`])\s*\1(?!`)/,
  br: /^ {2,}\n(?!\s*$)/,
  del: noop,
  text: /^[\s\S]+?(?=[\\<!\[_*`$]| {2,}\n|$)/,
  math: /^\$\$\s*([\s\S]*?[^\$])\s*\$\$(?!\$)/,
};
```

### 主题下开启mathjax
打开主题目录下的config.yml文件（注意这里是所使用主题的yml文件，不是hexo的yml文件）
加入下面这句：
```
mathjax: true
```

博客的时候需要开启Latex就需要加上头开启Mathjax
```
title: 护肤
categories:
  - 生活
tags:
  - tag1
date: 2021-10-07 15:57:57 
mathjax: true
```