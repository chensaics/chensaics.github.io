<section id="nice" data-tool="mdnice编辑器" data-website="https://www.mdnice.com"><p data-tool="mdnice编辑器">Python环境管理方法有很多工具，本文主要介绍Conda和pip的Python环境管理、更换国内镜像源加速下载的常用配置。</p>
<p data-tool="mdnice编辑器">互联网公开资料很多，可以自行搜索，也可以参考本文的一些设置。</p>
<h1 data-tool="mdnice编辑器"><span class="prefix"></span><span class="content">Conda 环境管理</span><span class="suffix"></span></h1>
<p data-tool="mdnice编辑器">1、安装<span class="footnote-word">anaconda3</span><sup class="footnote-ref">[1]</sup>后可以使用conda作为python环境管理工具。</p>
<p data-tool="mdnice编辑器">下载地址：</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">https://www.anaconda.com/products/individual<span class="hljs-comment">#Downloads</span><br></code></pre>
<p data-tool="mdnice编辑器">2、使用conda创建新环境，并指定python版本：</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">conda&nbsp;create&nbsp;-n&nbsp;环境名&nbsp;python=<span class="hljs-number">3.10</span><br></code></pre>
<p data-tool="mdnice编辑器">3、使刚刚创建的环境生效</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">conda&nbsp;activate&nbsp;环境名<br></code></pre>
<p data-tool="mdnice编辑器">4、查看现有环境</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">conda&nbsp;info&nbsp;-e<br>或<br>conda&nbsp;env&nbsp;list<br></code></pre>
<h1 data-tool="mdnice编辑器"><span class="prefix"></span><span class="content">conda 换国内镜像源</span><span class="suffix"></span></h1>
<p data-tool="mdnice编辑器">如需换成国内镜像源，提高下载速度，可以将pip/conda的配置中换镜像源。</p>
<h2 data-tool="mdnice编辑器"><span class="prefix"></span><span class="content">更换清华镜像源示例</span><span class="suffix"></span></h2>
<p data-tool="mdnice编辑器">各系统都可以通过修改<strong>用户目录下的 .condarc 文件</strong>来换<span class="footnote-word">清华镜像源</span><sup class="footnote-ref">[2]</sup>的。</p>
<p data-tool="mdnice编辑器">在.condarc文件中添加:</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">channels:<br>&nbsp;&nbsp;-&nbsp;defaults<br>show_channel_urls:&nbsp;<span class="hljs-literal">true</span><br>default_channels:<br>&nbsp;&nbsp;-&nbsp;https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main<br>&nbsp;&nbsp;-&nbsp;https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r<br>&nbsp;&nbsp;-&nbsp;https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2<br>custom_channels:<br>&nbsp;&nbsp;conda-forge:&nbsp;https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud<br>&nbsp;&nbsp;msys2:&nbsp;https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud<br>&nbsp;&nbsp;bioconda:&nbsp;https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud<br>&nbsp;&nbsp;menpo:&nbsp;https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud<br>&nbsp;&nbsp;pytorch:&nbsp;https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud<br>&nbsp;&nbsp;pytorch-lts:&nbsp;https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud<br>&nbsp;&nbsp;simpleitk:&nbsp;https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud<br></code></pre>
<p data-tool="mdnice编辑器">即可添加 Anaconda Python 免费仓库。</p>
<p data-tool="mdnice编辑器">Windows 用户如果无法直接创建名为 .condarc 的文件，可在终端先执行这条命令生成该文件之后再修改：</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">conda&nbsp;config&nbsp;--set&nbsp;show_channel_urls&nbsp;yes&nbsp;<br></code></pre>
<h2 data-tool="mdnice编辑器"><span class="prefix"></span><span class="content">常用命令</span><span class="suffix"></span></h2>
<p data-tool="mdnice编辑器">将会显示conda的配置信息：</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">conda&nbsp;config&nbsp;--show<br></code></pre>
<p data-tool="mdnice编辑器">删除原来的旧镜像</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">conda&nbsp;config&nbsp;--remove&nbsp;channels<br></code></pre>
<p data-tool="mdnice编辑器">添加清华镜像：</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">conda&nbsp;config&nbsp;--add&nbsp;channels&nbsp;https://mirrors.tuna.tsinghua.edu.cn。。。/地址见下面<br></code></pre>
<p data-tool="mdnice编辑器">设置搜索时显示通道地址：</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">conda&nbsp;config&nbsp;--set&nbsp;show_channel_urls&nbsp;yes<br></code></pre>
<p data-tool="mdnice编辑器">显示添加的源：</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">conda&nbsp;config&nbsp;--show&nbsp;channels<br></code></pre>
<p data-tool="mdnice编辑器">删除指定源：</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">conda&nbsp;config&nbsp;--remove&nbsp;channels&nbsp;源名称或链接<br></code></pre>
<h1 data-tool="mdnice编辑器"><span class="prefix"></span><span class="content">为pip配置国内镜像源</span><span class="suffix"></span></h1>
<h3 data-tool="mdnice编辑器"><span class="prefix"></span><span class="content">单次生效</span><span class="suffix"></span></h3>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">pip&nbsp;install&nbsp;【包名】&nbsp;-i&nbsp;https://mirrors.aliyun.com/pypi/simple/<br></code></pre>
<h3 data-tool="mdnice编辑器"><span class="prefix"></span><span class="content">配置pip镜像源(永久有效)</span><span class="suffix"></span></h3>
<ol data-tool="mdnice编辑器">
<li><section>Windows系统中pip换源:</section></li></ol>
<p data-tool="mdnice编辑器">windows下，直接在user目录中创建一个pip目录，如：</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">C:\Users\电脑用户名\pip\<br></code></pre>
<p data-tool="mdnice编辑器">新建文件，命名为：<strong>pip.ini</strong>，内容如下：</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">[global]&nbsp;<br>index-url&nbsp;=https://mirrors.aliyun.com/pypi/simple/<br></code></pre>
<ol start="2" data-tool="mdnice编辑器">
<li><section>Linux系统中pip换源:</section></li></ol>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">mkdir&nbsp;~/.pip<br><span class="hljs-built_in">cd</span>&nbsp;~/.pip<br>vim&nbsp;pip.conf<br><span class="hljs-comment">#&nbsp;添加下列内容：</span><br>[global]&nbsp;<br>index-url&nbsp;=https://mirrors.aliyun.com/pypi/simple/<br></code></pre>
<ol start="3" data-tool="mdnice编辑器">
<li><section>Mac中pip换源:</section></li></ol>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">vim&nbsp;~/.config/pip/pip.conf<br><span class="hljs-comment">#&nbsp;添加下列内容：</span><br>[global]&nbsp;<br>index-url=https://mirrors.aliyun.com/pypi/simple/<br>或者：<br>index-url=https://pypi.tuna.tsinghua.edu.cn/simple<br></code></pre>
<section class="footnotes-sep" data-tool="mdnice编辑器"></section>
<section class="footnotes" data-tool="mdnice编辑器">
<span id="fn1" class="footnote-item"><span class="footnote-num">[1] </span><p>anaconda3: <em>https://www.anaconda.com/products/individual#Downloads</em></p>
</span>
<span id="fn2" class="footnote-item"><span class="footnote-num">[2] </span><p>清华镜像源: <em>https://mirror.tuna.tsinghua.edu.cn/help/anaconda/</em></p>
</span>
</section>
</section>