---
  title: Matomo JavaScript 方式追踪单页面应用（SPA）
  date: 2018-12-19 21:23:13
  updated: 2018-12-23 16:57:38
  categories: ['前端'] 
  tags: ['Matomo','JavaScript','单页面']
  comments: true   
  keywords: Matomo,JavaScript,单页面   
  description: 最近在企业内网用 Matomo 做网站跟踪分析，有一个需求是跟踪手机端的应用（SPA应用）。研究了一番记录如下。
  img: 
---
单页面应用（[SPA](https://baike.baidu.com/item/SPA/17536313)）随着[Vue](https://cn.vuejs.org/ "Vue 官方网站")、[Angular](https://angular.io/ "angular 官网")、[React](https://react.docschina.org/ "中文文档")等框架的崛起，已经成为一种潮流。由于单页面应用自始至终都一个在同一个Docment上渲染元素，页面根本不跳转，这也让 Matomo 在默认用法下不能自动追踪。  
最近在企业内网用 Matomo 做网站跟踪分析，有一个需求是跟踪手机端的应用（SPA应用）。研究了一番记录如下。  

用JS方式追踪单页面应用，其原理就是当页面变动的时候，手动调用 Matomo 本来应该在页面加载时自动帮我们调用的几个API。

# 重新设置自定义变量 #
用下面的代码将 page 作用域的 自定义变量删除：
```js
_paq.push(['deleteCustomVariables', 'page']);
```

> 自定义变量有 page 作用域和 visit 作用域之分。page 作用域内的变量描述的是当前页面本身的属性，在每次页面请求的时候都可以变化，比如“文章分类”、“商品名称”之类的；visit 作用域内的变量在一个完整的session中只允许有一个值，这种作用域的变量适合存“访问者性别”，“访问者身份证号”等跟访问者有关的属性。关于自定义变量就说这么多，更多的信息可以先查看[官方文档](https://developer.matomo.org/guides/tracking-javascript-guide#custom-variables "“自定义变量” 官方文档")，我会尽快出一篇关于 Matomo [[自定义变量]](/2018/12/23/Matomo-Javascript-方式追踪-自定义变量/index.html "Matomo Javascript-方式追踪-自定义变量")
 和 自定义维度 的一篇文章。

# 设置页面加载时间 #
你可能会用 Ajax 从服务器实时加载新模块，那你得做一个计时，并且把加载这个模块的耗时（毫秒数）通过下面这个 API 发送给 Matomo 。
```js
_paq.push(['setGenerationTimeMs', timeItTookToLoadPage]);
```

# 设置引用页（Referer） #
告诉 Matomo 现在这个页面是从哪个页面跳过来的。
```js
_paq.push(['setReferrerUrl', previousPageUrl]);
```

>重要提示：
```js
_paq.push(['deleteCustomVariables', 'page']);
_paq.push(['setGenerationTimeMs', timeItTookToLoadPage]);
_paq.push(['setReferrerUrl', previousPageUrl]);
```
>上面的代码执行后需要执行`_paq.push(['trackPageView']);`才能提交修改。具体的可以看文末的例子
# 追踪新内容 #
当新的内容被加载到 Document 中，新内容这一块我们得让 Matomo 扫描。下面的一些代码不是必须的，根据你的需求来定。

如果新内容中有音视频，并且你用到了 Matomo 的[音视频分析](https://matomo.org/docs/media-analytics/)
```js
_paq.push(['MediaAnalytics::scanForMedia', documentOrElement]);
```
如果新内容中有表单，并且你用到了 Matomo 的[表单分析](https://matomo.org/docs/form-analytics/)
```js
_paq.push(['FormAnalytics::scanForForms', documentOrElement]);
```
如果新内容中有超链接，并且你用到了 Matomo 的离站链接分析、下载分析
```js
_paq.push(['enableLinkTracking']);
```
如果你用到了 Matomo 的[内容追踪](https://matomo.org/docs/content-tracking/)
```js
_paq.push(['trackContentImpressionsWithinNode', documentOrElement]);
```
---
如果你不想做那么多判断，你可以像下面的例子一样，一股脑全部调用一下，毕竟这也耗不了多少性能。

# 例子 #
```js
var currentUrl = location.href;
window.addEventListener('hashchange', function() {
    _paq.push(['setReferrerUrl', currentUrl]);
     currentUrl = '' + window.location.hash.substr(1);
    _paq.push(['setCustomUrl', currentUrl]);
    _paq.push(['setDocumentTitle', 'My New Title']);

    // remove all previously assigned custom variables, requires Matomo (formerly Piwik) 3.0.2
    _paq.push(['deleteCustomVariables', 'page']); 
    _paq.push(['setGenerationTimeMs', 0]);
    _paq.push(['trackPageView']);

    // make Matomo aware of newly added content
    var content = document.getElementById('content');
    _paq.push(['MediaAnalytics::scanForMedia', content]);
    _paq.push(['FormAnalytics::scanForForms', content]);
    _paq.push(['trackContentImpressionsWithinNode', content]);
    _paq.push(['enableLinkTracking']);
});

```