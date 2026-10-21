---
title: 第一张
date: 2024-01-19 11:52:35
categories: 时间序列处理
mathjax: true
description: 记录第一次用 hexo 搭博客的后半段过程，主要是各种参数的设置
---

## Hexo 初步部署

趁还有印象，记一下第一次用 Hexo 搭博客时可能要注意的内容；方法本身是从知乎和 csdn 以及博客园上的文章中学来的，但是似乎完全照搬并不能解决问题，需要根据具体情况对有些地方做略微改动，所以这里写的也就算大杂烩了。当然，看起来还是大同小异的

### 路径设置

github 现在创建默认分支时需要键入

```
git branch -M main
```

默认分支名称就成了 main ，所以需要在博客文件夹根目录下的 _config.yml 文件里在尾端要添上如下内容

```yaml
deploy:
  type: git
  repository: https://github.com/PMUGO50/pmugo50.github.io.git
  branch: main
```

就添在下列注释语句的后一行即可

```yaml
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
```

同时在下列注释语句

```yaml
# URL
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
```

的后面要添加正确的 url 和 root 路径，这里 url 即为博客网站的域名

```yaml
url: https://pmugo50.github.io
root: /
```

### 初步主题设置

在根目录的 _config.yml 文件中，下列注释语句

```yaml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
```

的后面添加上想用的主题，以 next 主题为例

```yaml
theme: next
```

当然，在此之前要将主题文件下载到根目录下的 themes 文件夹里

然后在相应主题文件夹里的，比如 \themes\next 中还有主题的 _config.yml 文件，在其中可以对主题进行具体的定制，在一开始主要是选择主题的大致样式。找到

```yaml
# Schemes
#scheme: Muse
#scheme: Mist
#scheme: Pisces
#scheme: Gemini
```

next 提供了四种渲染方案，想用哪个就把其注释拿掉打开就行，**注意不要同时拿掉两个** 

### 初步主页设置

在根目录的 _config.yml 文件的开头没几行，可以看到 #site 的注释语句，在其下面就是初步的主页内容设置，可以个性化定制，比如

```yaml
title: PhecStar
subtitle: ''
description: 对今天而言，最宝贵的是对明天的希望
keywords:
author: PMUG050
language: zh-CN
timezone: Asia/Shanghai
```

同时也可以决定侧边栏要放置些什么，在\themes\next 中的 _config.yml 文件里找到

```yaml
menu:
  home: / || fa fa-home
  #about: /about/ || fa fa-user
  #tags: /tags/ || fa fa-tags
  #categories: /categories/ || fa fa-th
  archives: /archives/ || fa fa-archive
  #schedule: /schedule/ || fa fa-calendar
  #sitemap: /sitemap.xml || fa fa-sitemap
  #commonweal: /404/ || fa fa-heartbeat
```

需要侧边栏展示哪个就打开哪个的注释，然后比如说我展示了首页、关于、分类和归档（印象中首页和归档好像是默认启用的），就在根目录下启动 git bash ，输入

```
hexo new page "about"
hexo new page "categories"
```

这样在**根目录的 source 文件夹**里就多出来 about 和 categories 两个文件夹，其中分别有 index.md 文件，可以对其进行修改，比如对于 categories 可以写入

```markdown
---
title: 分类
date: 2024-01-19 17:57:08
type: "categories"
comments: false
---
```

comments: false 代表关闭评论；对于 about ，除了两个 --- 之间的内容，还可以在下面正文区域添加想要显示的内容，比如对站点进行简单说明

这样初步的网页就成形了

### 部署网页

在博客文件夹根目录下打开 git bash，输入

```
hexo clean
hexo g
hexo d
```

即可部署网页，再输入

```
hexo s
```

此时会返回一个网址，点进去就能查看网页的效果，查看完后按 ctrl+c 退出查看

---

本部分过程参考了

- https://www.zhihu.com/question/335470115

- https://www.zhihu.com/column/c_1201860091307458560

- https://blog.csdn.net/qq_40871466/article/details/86763540

---

## 公式、图片和字体

### 字体大小

> em类似一种相对单位，对于浏览器的默认设定，一般有 1em = 16px

仍然用 next 主题，在**主题文件夹，即 \themes\next 里**的 _config.yml 中找到

```yaml
font:
  enable: true

  # Uri of fonts host, e.g. https://fonts.googleapis.com (Default).
  host: https://fonts.googleapis.com

  # Font options:
  # `external: true` will load this font family from `host` above.
  # `family: Times New Roman`. Without any quotes.
  # `size: x.x`. Use `em` as unit. Default: 1 (16px)

  # Global font settings used for all elements inside <body>.
  global:
    external: true
    family: Lato
    size: 0.95

  # Font settings for site title (.site-title).
  title:
    external: true
    family:
    size: 2

  # Font settings for headlines (<h1> to <h6>).
  headings:
    external: true
    family:
    size: 1.625

  # Font settings for posts (.post-body).
  posts:
    external: true
    family:
    size: 0.95

  # Font settings for <code> and code blocks.
  codes:
    external: true
    family:
    size: 0.85
```

将 font 下的 enable 置为 true，host 可以采用想要的字体的网址，后面的 global ，title ，headings ，posts ，codes 中

- external 表示是否采用 host 中的字体
- family 表示采用新字体
- size 则表示该部分的字体的基本大小；这其中 headings 字体大小会随标题级数的增加而减小

然后在 \themes\next\source\css\_variables\base.styl 中找到

```stylus
// Font size
$font-size-base           = (hexo-config('font.enable') and hexo-config('font.global.size') is a 'unit') ? unit(hexo-config('font.global.size'), em) : .95em;
$font-size-smallest       = .8em;
$font-size-smaller        = .85em;
$font-size-small          = .9em;
$font-size-medium         = .95em;
$font-size-large          = 1em;
$font-size-larger         = 1.25em;
$font-size-largest        = 1.45em;

// Headings font size
$font-size-headings-step  = .125em;
$font-size-headings-base  = (hexo-config('font.enable') and hexo-config('font.headings.size') is a 'unit') ? unit(hexo-config('font.headings.size'), em) : 1.625em;
```

修改等号后面的值可以进一步改变页面各部分对应的字号

### mathjax 公式支持

1. hexo 默认的 math 支持插件是 hexo-math 以及 hexo-renderer-marked ，将二者卸载并分别换为hexo-filter-mathjax 以及 hexo-renderer-kramed ，即在根目录下打开 git bash ，输入

   ```
   npm uninstall hexo-renderer-marked --save
   npm uninstall hexo-math --save
   npm install hexo-filter-mathjax --save
   npm install hexo-renderer-kramed --save
   ```

   下载好后可以通过 npm list 命令确定是否已安装成功

   ```
   npm list hexo-filter-mathjax
   npm list hexo-renderer-kramed
   ```

2. 接下来在**主题文件夹，即 \themes\next 里**的 _config.yml 中找到

   ```yaml
   math:
     # Default (true) will load mathjax / katex script on demand.
     # That is it only render those page which has `mathjax: true` in Front-matter.
     # If you set it to false, it will load mathjax / katex srcipt EVERY PAGE.
     per_page: true
   
     # hexo-renderer-pandoc (or hexo-renderer-kramed) required for full MathJax support.
     mathjax:
       enable: true
       # See: https://mhchem.github.io/MathJax-mhchem/
       mhchem: true
   ```

   将 mathjax 下两项都置为 true

3. 然后为了改掉一些符号冲突，打开**根目录**下的 node_modules\kramed\lib\rules\inline.js ，将其中的

   ```js
   escape: /^\\([\\`*{}\[\]()#$+\-.!_>])/,
   em: /^\b_((?:__|[\s\S])+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
   ```

   两行分别修改为

   ```js
   escape: /^\\([`*\[\]()#$+\-.!_>])/,
   em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
   ```

4. 最后，在每次写 blog 的 md 文档的时候，在开头的导言区添上

   ```
   mathjax: true
   ```

### 图片插入与大小

1. 安装插件 hexo-asset-image ，在根目录下打开 git bash ，输入

   ```
   npm install https://github.com/CodeFalling/hexo-asset-image --save
   ```

2. 将**根目录**下的 _config.yml 文件中的 post_asset_folder 置为 true ，这样每次用 hexo new 指令生成新 blog 文档时会自动产生一个同名文件夹，其中就可以放图片

   比如说此时 blog 文档名称为 w123.md ，生成的图片文件夹则为 w123

3. 在 md 文档中写入图片时，采用 html 的写法并用相对路径，但是注意最前面 "./" 的存在；这里 width 的比例指定，height 缺省时会自动保证图片比例不变，宽度变为页面宽度的 65%

   ```markdown
   <img src="./w123/pic1.png" width="65%" />
   ```
   
### 链接蓝色化

在 \themes\next\source\css\_common\components\post\post.styl 的最后插入

```stylus
.post-body a:not(.btn){
     color: #0593d3;
     border-bottom: none;
     &:hover {
       color: #0477ab;
       text-decoration: underline;
     }
   }
```

---

本部分过程参考了

- https://blog.csdn.net/m0_43401436/article/details/107191688

- https://zhuanlan.zhihu.com/p/265077468

- https://zhuanlan.zhihu.com/p/106060640

- https://blog.csdn.net/weixin_45511189/article/details/115798563

- https://blog.csdn.net/Growing_potato/article/details/113926894

---

## 背景美化等

### 设置背景图片

在 \themes\next 里的 _config.yml 中找到

```yaml
#style: source/_data/styles.styl
```

将其注释取消

然后在**根目录下的 source** 文件夹下创建 _data 文件夹，并在其中创建 styles.styl 文件，向文件中写入

```stylus
body {
      background: url(/images/xxx.png);
      background-size: cover;
      background-repeat: no-repeat;
      background-attachment: fixed;
      background-position: 50% 50%;
}
```

然后在**根目录下的 source** 文件夹下再创建 images 文件夹，将背景图片放入其中并命名 xxx.png（xxx 可以随便换，只要保证和 styl 文件中一致即可）就设置完毕了

### 设置透明度

打开上一步创建的 styles.styl 文件，向文件中写入

```stylus
.content-wrap {
  opacity: 0.8;
}

.sidebar {
  opacity: 0.9;
}

.header-inner {
  background: rgba(255,255,255,0.8);
}

.popup {		
  opacity: 0.9;
}
```

这里 content-wrap 是主题内容部分的背景，sidebar 是侧边栏的背景，header-inner 是菜单栏的背景，popup 是搜索框

修改 opacity 的值即可改变透明度，从 0 到 1 是从完全透明到完全不透明

### 创建 Github 和邮箱链接

在 \themes\next 里的 _config.yml 文件中找到 social 字段，然后下面将会有

```yaml
  #GitHub: https://github.com/yourname || fab fa-github
  #E-Mail: mailto:yourname@gmail.com || fa fa-envelope
  #Weibo: https://weibo.com/yourname || fab fa-weibo
  #Google: https://plus.google.com/yourname || fab fa-google
  #Twitter: https://twitter.com/yourname || fab fa-twitter
  #FB Page: https://www.facebook.com/yourname || fab fa-facebook
  #StackOverflow: https://stackoverflow.com/yourname || fab fa-stack-overflow
  #YouTube: https://youtube.com/yourname || fab fa-youtube
  #Instagram: https://instagram.com/yourname || fab fa-instagram
  #Skype: skype:yourname?call|chat || fab fa-skype
```

需要展示哪些联系方式就打开哪个注释，我打开的是 github 和邮箱

关于邮箱，在文件中填写完后，打开网页并点击，它会定向到 localhost:4000/youremail@xxx.com 这个网址。为此，可以做新的一页来放入邮箱地址

首先，在上面的 E-mail 字段里不填写邮箱地址而是随便一串字母，比如

```yaml
E-Mail: emailaddress || fa fa-envelope
```

然后，在根目录下启动 git bash ，输入

```
hexo new page "emailaddress"
```

此时在根目录下的 source 文件夹下出现 emailaddress 文件夹，向其中的 index.md 文件中写入邮箱内容即可，比如我写入的是

```markdown
---
title: E-mail
date: 2024-01-31 22:45:13
comments: false
---

<center>
    Outlook: <a href="mailto:yyq26314@outlook.com">yyq26314@outlook.com</a> <br />
    QQ: <a href="mailto:1480062774@qq.com">1480062774@qq.com</a>
    <center>
```

这样再点击网页上的按钮，就会跳转到这个页面了

---

本部分过程参考了

- https://blog.csdn.net/weixin_53070478/article/details/122328889

- https://blog.csdn.net/qq_29119593/article/details/81132471

---
