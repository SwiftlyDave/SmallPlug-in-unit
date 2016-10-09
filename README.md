<p>Small 插件化调研、学习、示例应用</p>

<h2><a id="user-content-工程模块" class="anchor" href="#工程模块" aria-hidden="true"><svg aria-hidden="true" class="octicon octicon-link" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>工程模块</h2>

<p>app : 宿主模块<br>
lib.style : styles库，包括公共的strings、colors、dimens等<br>
lib.framework : 基本框架库，包括网络请求、缓存、动态代理回调、Utils等。<br>
app.main : App的主模块<br>
app.phone : 查询手机号码归属地模块<br>
app.weather : 查询北京天气模块<br>
app.shanghai.weather : 查询上海天气模块  </p>

<h2><a id="user-content-写在前面的话" class="anchor" href="#写在前面的话" aria-hidden="true"><svg aria-hidden="true" class="octicon octicon-link" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>写在前面的话</h2>
<article class="post-content">
  <p>这两年热修复、组件化、插件化很火，火到中国这方面的开源项目遍地开花，例如：屠毅敏的AndroidDynamicLoader、任玉刚的dynamic-load-apk、张勇的DroidPlugin、阿里的AndFix、林光亮的Small等，除了中国这些热修复、插件化的开源项目，你有听过外国的嘛。虽然你可能看过这样的文章《插件化从入门到放弃》，但你是否还看过这样的文章《插件化从放弃到捡起》，尽管应用热修复和插件化坑多、难度高，但我们还是一往情深、纵身向前，因为她的优点远多于她的缺点。</p>

<h2 id="section">插件化在实际开发中的主要好处有：</h2>
<p>1、团队插件化开发并可以单独编译运行。<br />
2、如果插件出问题可以动态更新插件进行补救。<br />
3、根据业务需求增加和去掉相关插件。</p>

<p>这半年多我已经尝试应用了阿里的AndFix、张勇的DroidPlugin和林光亮的Small，最终决定在公司的项目中应用Small框架，因为它让开发更轻巧、简单、清晰的与插件无缝结合。下面具体说说Small的使用与相关问题，先看下作者给出的Small最佳实践。</p>

<h2 id="section-1">基本原则</h2>
<p>宿主中不要放业务逻辑。只做加载插件以及调起主插件的操作。</p>

<h2 id="section-2">重构步骤</h2>
<p>Android工程的组件一般分为两种：lib组件和application组件。</p>

<h4 id="lib-">拆 <code class="highlighter-rouge">lib.*</code> ：公共库模块插件</h4>
<p>把各个第三方库拆出来做成一个个 <code class="highlighter-rouge">lib.*</code> 库模块插件，包括统计、地图、网络、图片、支付、分享等库。<br />
把老项目积累的业务公共代码(utils)分离出来封装成一个 lib.utils 插件。<br />
把基础的样式、主题分离出来封装成一个 lib.style 插件。</p>

<h4 id="app-">拆 <code class="highlighter-rouge">app.*</code> ：业务模块插件</h4>
<p>把业务模块拆成 <code class="highlighter-rouge">app.*</code> 业务模块插件，他们可以依赖 <code class="highlighter-rouge">lib.*</code> 模块，显示调用 <code class="highlighter-rouge">lib.*</code> 中的各个API。<br />
相对独立的业务模块先拆，比如“详情页”、“关于我们”，如果剩下的业务不好拆，先放一个插件里。<br />
如果都不好拆，先把全部业务做成一个 app.main 主插件。</p>

<h2 id="small">Small的使用与注意</h2>
<p>Small的具体使用还请参考<a href="https://github.com/wequick/Small/tree/master/Android">GitHub 上 Small 的 Android 应用</a>。<br />
需要注意的是模块的命名：<br />
模块命名如：<code class="highlighter-rouge">app.*</code>，<code class="highlighter-rouge">lib.*</code>，<code class="highlighter-rouge">web.*</code><br />
包名命名如：<code class="highlighter-rouge">宿主包名.app.*</code>，<code class="highlighter-rouge">宿主包名.lib.*</code>，<code class="highlighter-rouge">宿主包名.web.*</code><br />
因为Small会根据包名对插件进行归类，特殊的域名空间如：“.app.” 会让这变得容易。<br />
从Small的组件打包源码中(buildSrc/…/PluginType)能看到该框架是对插件进行归类的：</p>

<div class="highlighter-rouge"><pre class="highlight"><code>public enum PluginType {
    Unknown (0),
    Host    (1),
    App     (2),
    Library (3),
    Asset   (4)

    private int value
    public PluginType(int value) {
        this.value = value
    }
}
</code></pre>
</div>

<h2 id="small-1">Small框架的源码概括</h2>

<p>在 GitHub 上 Small/DevSample 下可以看到Small包涵两套源码：<br />
buildSrc：组件编译插件，用于打包安卓组件包，包括分离宿主和公共库的类与资源、签名组建包、通过脚本的方式来解决资源冲突等问题。<br />
small：核心库，用于加载安卓组件包，包括动态加载类、动态加载资源、通过占坑Activity达到动态注册Activity的效果等。</p>

<h2 id="section-3">业务模块和库模块的编译命令</h2>

<p>编译 app.* 模块： ./gradlew buildBundle -q<br />
清除 app.* 数据： ./gradlew cleanBundle</p>

<p>编译 lib.* 库： ./gradlew buildLib -q<br />
清除 lib.* 数据： ./gradlew cleanLib</p>

<h2 id="small-2">Small应用记录</h2>

<p>1、lib.* 与 lib.* 之间不支持资源的引用；app.* 可以引用 lib.* 下的资源。</p>

<p>2、如果 lib.* 下资源变化特别是删除资源，也请删掉 lib.* 下 public.txt 资源编号集合。</p>

<p>3、第三方SDK的 meta、Service 等需要在宿主 App 的 manifest 中声明。</p>

<p>4、模块之间跳转有两种方式</p>

<div class="highlighter-rouge"><pre class="highlight"><code>(1). Small.openUri("phone/Number?num=18600604600", mContext);  
(2). Intent intent = Small.getIntentOfUri("phone/Number", mContext);  
     intent.putExtra("num", "18600604600");  
     startActivity(intent);  
     不过第二种方式还有问题（Small稳定版本：0.9）  
</code></pre>
</div>

<p>5、获取传递的参数也是通过 Small 实现的</p>

<div class="highlighter-rouge"><pre class="highlight"><code>Uri uri = Small.getUri(this);  
String num = uri.getQueryParameter("num");  
</code></pre>
</div>

<p>6、回传数据简单些不需要 startActivityForResult(Intent intent, int requestCode)，直接返回就好</p>

<div class="highlighter-rouge"><pre class="highlight"><code>Intent intent = new Intent();  
intent.putExtra("result", "XXOO");  
setResult(requestCode, intent);  
建议传递 requestCode，将 requestCode 做为 resultCode 返回。  
</code></pre>
</div>

<h2 id="section-4">插件升级策略</h2>

<p>测试发现 Small 在前台更新完插件，重新启动 App 新的插件功能才会生效，
如果不重新启动插件而继续使用插件，程序可能会崩掉，可以看出 Small 还不支持热更新。</p>

<p>所以我的策略是App启动时检查是否需要更新插件，更新插件的 <a href="http://sunfusheng.com/assets/small/updates.json">updates.json</a> 数据，增加插件的 <a href="http://sunfusheng.com/assets/small/additions.json">additions.json</a> 数据。这时启动的是 IntentService 服务，IntentService 的优点在这里可以充分显示出来。
有关 IntentService 的使用请参考 <a href="http://sunfusheng.com/android/2016/07/01/IntentService.html">IntentService 示例与详解</a>。如果需要更新插件，则再次启动服务，下载最新插件，下载完毕后，服务会自动停止。当退出 App 时再次启动服务检查是否需要更新插件（updateBundles），需要的话将插件合并到宿主包中，再次启动 App 时就可以看到最新的功能啦。</p>

<p><a href="https://github.com/sfsheng0322/DroidSmall">GitHub上示例工程DroidSmall</a><br />
