title: wordpress迁移到Hexo遇到的问题
date: 2015-08-19 16:16:44
tags:
  - Hexo
  - Wordpress
categories:
  - CS
---


1. wordpress内容导出不完全  
可能是服务器主机的内存有限？  
最后在本地搭了个wordpress环境，并导入数据库。然后再本地的wordpress dashboard中进行导出，成功。

2. 运行hexo migrate wordpress (source)出错  
JS-YAML: can not read a block mapping entry; a multiline key may not be an implicit key at line 2, column 5:  
date: 2012-09-30 12:58:01  
^  
是因为某一篇文章的title中有引号导致的，去除引号之后，搞定

3. 运行hexo generate出错  <!--more-->
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html  
Template render error: expected variable end  
at Error.exports.TemplateError (d:\lsharemy\node_modules\hexo\node_modules\nunjucks\src\lib.js:51:19)  
at Object.extend.fail (d:\lsharemy\node_modules\hexo\node_modules\nunjucks\src\parser.js:64:15)  
at Object.extend.advanceAfterVariableEnd (d:\lsharemy\node_modules\hexo\node_modules\nunjucks\src\parser.js:133:18)  
at Object.extend.parseNodes (d:\lsharemy\node_modules\hexo\node_modules\nunjucks\src\parser.js:1159:22)  
at Object.extend.parseAsRoot (d:\lsharemy\node_modules\hexo\node_modules\nunjucks\src\parser.js:1177:42)  
at Object.module.exports.parse (d:\lsharemy\node_modules\hexo\node_modules\nunjucks\src\parser.js:1199:18)  
at Object.module.exports.compile (d:\lsharemy\node_modules\hexo\node_modules\nunjucks\src\compiler.js:1118:48)  
at Obj.extend._compile (d:\lsharemy\node_modules\hexo\node_modules\nunjucks\src\environment.js:444:35)  
at Obj.extend.compile (d:\lsharemy\node_modules\hexo\node_modules\nunjucks\src\environment.js:433:18)  
at null.< anonymous> (d:\lsharemy\node_modules\hexo\node_modules\nunjucks\src\environment.js:378:22)  
at Object.exports.withPrettyErrors (d:\lsharemy\node_modules\hexo\node_modules\nunjucks\src\lib.js:24:16)  
at Obj.extend.render (d:\lsharemy\node_modules\hexo\node_modules\nunjucks\src\environment.js:374:20)  
at Obj.extend.renderString (d:\lsharemy\node_modules\hexo\node_modules\nunjucks\src\environment.js:261:21)  
at d:\lsharemy\node_modules\hexo\lib\extend\tag.js:56:9  
at tryCatcher (d:\lsharemy\node_modules\hexo\node_modules\bluebird\js\main\util.js:26:23)  
at Promise._resolveFromResolver (d:\lsharemy\node_modules\hexo\node_modules\bluebird\js\main\promise.js:476:31)  
at new Promise (d:\lsharemy\node_modules\hexo\node_modules\bluebird\js\main\promise.js:69:37)  
at Tag.render (d:\lsharemy\node_modules\hexo\lib\extend\tag.js:55:10)  
at Object.tagFilter [as onRenderEnd] (d:\lsharemy\node_modules\hexo\lib\hexo\post.js:253:16)  
at Promise.then.then.then.output (d:\lsharemy\node_modules\hexo\lib\hexo\render.js:55:19)  
at tryCatcher (d:\lsharemy\node_modules\hexo\node_modules\bluebird\js\main\util.js:26:23)  
at Promise._settlePromiseFromHandler (d:\lsharemy\node_modules\hexo\node_modules\bluebird\js\main\promise.js:503:31)  
at Promise._settlePromiseAt (d:\lsharemy\node_modules\hexo\node_modules\bluebird\js\main\promise.js:577:18)  
at Promise._settlePromises (d:\lsharemy\node_modules\hexo\node_modules\bluebird\js\main\promise.js:693:14)  
at Async._drainQueue (d:\lsharemy\node_modules\hexo\node_modules\bluebird\js\main\async.js:123:16)  
at Async._drainQueues (d:\lsharemy\node_modules\hexo\node_modules\bluebird\js\main\async.js:133:10)  
at Async.drainQueues (d:\lsharemy\node_modules\hexo\node_modules\bluebird\js\main\async.js:15:14)  
at process._tickCallback (node.js:442:13)  
删除掉部分文章后，就可以生成成功，说明应该是某些文章的解析出了问题
usaco-prob-party-lamps.md  
int d[4][2] = { {1, 1}, {1, 2}, {2, 2}, {1, 3} }; 多维数组定义，背hexo认为是变量，然后出错了。  
加两个空格搞定。

4. url不想变  
http://lsharemy.com/wordpress/index.php/lmy/20121102-20130908/  
比如原来一篇文章的链接是这样的  
后来想到可以将wordpress和index.php认为是一个类别，然后配置permalink: :category/:title来达到目的  
不过还有个小问题，Hexo中会默认将.替换成-，也就是变成了index-php。
后来直接改slugize的源文件，让其不要转义.  
\node_modules\hexo\node_modules\hexo-util\lib\slugize.js  
post的链接算是搞定

5. 某些googlg服务被墙导致的速度慢的问题  
\themes\landscape\source\css\_variables.styl
24行，font-mono中，删除"Source Code Pro"字体  
\themes\landscape\layout\_partial\head.ejs  
删除< link href="//fonts.googleapis.com/css?family=Source+Code+Pro" rel="stylesheet" type="text/css">  
\themes\landscape\layout\_partial\after-footer.ejs  
将< script src="//ajax.googleapis.com/ajax/libs/jquery/2.0.3/jquery.min.js">< /script>  
替换为  
< script src="http://libs.baidu.com/jquery/2.0.3/jquery.min.js">< /script>  
这样之后，访问速度快了很多  

6. 部署到github  
`npm install hexo-deployer-git --save`


    deploy:  
        type: git  
        repository: https://github.com/lsharemy/lsharemy.github.io.git
        branch: master  
现在type是git，如果填github，会提示ERROR Deployer not found: github

7. cname  
npm install hexo-generator-cname --save

8. 百度统计  
将统计代码放在themes/landscape/layout/_partial/header.ejs中  
< /head >标签前

9. 评论  
Disqus  
wordpress直接可以导入  
有个小问题就是，对于page的评论匹配不了。  
后来发现是Hexo对于post和page的permalink的格式有点不一样
page的permalink的结尾有/index.html，而post没有  
wordpress导出的xml中，是没有/index.html的  
本来想改Hexo的代码的，后来怕改出什么问题，于是修改了wordpress导出的xml，在page的链接后面都加上了/index.html
