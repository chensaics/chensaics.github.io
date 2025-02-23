---
title: 'RTX4090 + Cuda'
date: 2025-02-03
permalink: /posts/2025/02/RTX4090-Cuda/
tags:
  - linux
---

本文主要内容
==============

- 硬件参考（哪里省钱，哪里不要省）
- Win10+RTX4090深度学习环境配置
- Mac远程联机Windows（ssh、APP

这两天买了一张<strong>RTX4090的显卡</strong>，就在以前的主机上安装和配置了一下，把过程记录下来，有需要的人可以参考一下，先上一个成品图：
<img src="https://files.mdnice.com/user/38578/2e74eacf-b092-43af-b75b-26397b24c85d.jpg" alt="">


硬件
==========
<p data-tool="mdnice编辑器">主机是2020年买的，可以根据需要买更高配的。
<img src="https://files.mdnice.com/user/38578/1c866290-3ef6-494b-b103-489a638676d3.png" alt="">
本次要更换的就是红框中的三个，显卡、内存、电源，其他的都还能用。</p>
<h2 data-tool="mdnice编辑器"><span class="prefix"></span><span class="content">硬件该怎么选？</span><span class="suffix"></span></h2>
<p data-tool="mdnice编辑器">这个我朋友也给了我很多的参考建议，包括各个硬件都换一遍如何选，也参考了<span class="footnote-word">知乎@良睦路程序员</span><sup class="footnote-ref">[1]</sup>的回答。</p>
<p data-tool="mdnice编辑器">为了不花冤枉钱还能达到目标，我就换了这三个
<img src="https://files.mdnice.com/user/38578/34fe19b4-2ff4-48f3-a694-4a0a0a9d569c.png" alt=""></p>
<p data-tool="mdnice编辑器">想换4090 24G显卡，电源肯定要跟上（换成了1000w以上），内存原来16G也是短板，这次换成32G的。机箱能放得下显卡（长度要注意）
<img src="https://files.mdnice.com/user/38578/b361d364-8391-40fc-8e78-cb278f402927.jpg" alt=""></p>
<p data-tool="mdnice编辑器">这里也收集了一些显卡的参数对比，仅供参考。
涡轮卡一些参数：
<img src="https://files.mdnice.com/user/38578/a80ecb7d-6db8-4561-b483-d5d863ad7d29.png" alt="">
公版卡参数（难买到）：
<img src="https://files.mdnice.com/user/38578/3a5acd56-4035-4d9a-821c-10316de62cb8.png" alt=""></p>
<h2 data-tool="mdnice编辑器"><span class="prefix"></span><span class="content">如何买？</span><span class="suffix"></span></h2>
<p data-tool="mdnice编辑器">显卡毕竟很贵，最担心是买到矿卡，我朋友建议的品牌：影驰、技嘉、微星，不着急可以等活动省大几百块。</p>
<p data-tool="mdnice编辑器">不要去二手店、组装店之类的，很可能会买到二手卡。</p>
<p data-tool="mdnice编辑器">同一品牌下的同一款卡，还会配不同套装，比如水冷、RGB灯之类的，根据自己喜好和预算选一个即可。</p>
<h1 data-tool="mdnice编辑器"><span class="prefix"></span><span class="content">深度学习环境配置</span><span class="suffix"></span></h1>
<p data-tool="mdnice编辑器">我们拆机组装好后【如果不知道如何装机，自行搜视频看哈】，就开始安装一些软件了。</p>
<ul data-tool="mdnice编辑器">
<li><section>如何看显卡安装成功？</section></li></ul>
<p data-tool="mdnice编辑器">在<strong>设备管理器</strong>中可以查看：
<img src="https://files.mdnice.com/user/38578/672fe722-77a4-4c5d-8910-4fdea977ea61.png" alt=""></p>
<h2 data-tool="mdnice编辑器"><span class="prefix"></span><span class="content">显卡驱动安装：</span><span class="suffix"></span></h2>
<p data-tool="mdnice编辑器">要在Windows上使用4090做模型训练，一方面要下载<span class="footnote-word">显卡驱动</span><sup class="footnote-ref">[2]</sup>：<a href="https://www.nvidia.cn/geforce/drivers/">https://www.nvidia.cn/geforce/drivers/</a>，另外要训练神经网络我们要安装CUDAtoolkit和cuDNN。</p>
<p data-tool="mdnice编辑器">如图：
<img src="https://files.mdnice.com/user/38578/4d26bee8-c245-4fdb-96dc-f30679cf5378.png" alt="">
根据自己的系统和显卡型号搜索一下即可。</p>
<p data-tool="mdnice编辑器">经验告诉我<strong>不要使用最新的版本</strong>，dddd(懂的都懂)。4090显卡需要“驱动程序版本: 526.98 - 发行日期: 2022-11-16”<strong>及以上</strong>。</p>
<p data-tool="mdnice编辑器">下载后，根据推荐<strong>一路点下去</strong>安装。不用管界面显示哪个路径，那个是临时的，最终会自动删除，并最终安装在默认路径：</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">C盘的&nbsp;NVIDIA&nbsp;Corporation<br>C盘的&nbsp;NVIDIA&nbsp;GPU&nbsp;Computing&nbsp;Toolkit<br></code></pre>
<p data-tool="mdnice编辑器">这时候，可以打开终端（win + R 快捷键），输入cmd，终端输入 nvidia-smi 查看：</p>
<figure data-tool="mdnice编辑器"><img src="https://files.mdnice.com/user/38578/20541106-d2b5-494c-a27a-4cb1d3f71783.png" alt=""></figure>
<p data-tool="mdnice编辑器">这里哪怕是12.x版本也没事，我们可以使用conda管理我们的项目环境，只需要那里的cudatoolkit和cudnn版本一致即可。原因下文有解释。</p>
<h2 data-tool="mdnice编辑器"><span class="prefix"></span><span class="content">cudatoolkit和cudnn 安装</span><span class="suffix"></span></h2>
<p data-tool="mdnice编辑器">也可以单独再下载 11.8版本的<span class="footnote-word">cudatoolkit</span><sup class="footnote-ref">[3]</sup>和对应的<span class="footnote-word">cuDNN</span><sup class="footnote-ref">[4]</sup>：
<a href="https://developer.nvidia.com/cuda-toolkit-archive">https://developer.nvidia.com/cuda-toolkit-archive</a></p>
<p data-tool="mdnice编辑器"><a href="https://developer.nvidia.com/rdp/cudnn-download">https://developer.nvidia.com/rdp/cudnn-download</a></p>
<p data-tool="mdnice编辑器">一路点击安装即可。</p>
<p data-tool="mdnice编辑器"><img src="https://files.mdnice.com/user/38578/89340968-fc48-457c-8a78-6d69e210544b.png" alt="">
cuDNN的下载需要注册，挺麻烦，11.8版本的我上传到网盘了，文末可以获取连接哈。</p>
<p data-tool="mdnice编辑器">下载后解压，<strong>对应目录的文件，复制到</strong>刚刚安装cuda toolkit<strong>同名文件夹下</strong>即可。</p>
<ul data-tool="mdnice编辑器">
<li><section>在终端输入 nvcc -V
<img src="https://files.mdnice.com/user/38578/c4053186-3ac0-45b2-abc1-e8e6b7c8430f.png" alt=""></section></li></ul>
<p data-tool="mdnice编辑器">如果没有正常显示，可以查看系统环境变量：</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">**win+i快捷键**&nbsp;打开配置，<br>依次点击<span class="hljs-string">"系统"</span>、<span class="hljs-string">"关于"</span>、<span class="hljs-string">"高级系统设置"</span>就能打开系统属性，<br>在“高级”下点击“环境变量N”，<br>在系统变量中找到path并双击。&nbsp;<br></code></pre>
<p data-tool="mdnice编辑器">以下如果没有就添加：</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">C:\Program&nbsp;Files\NVIDIA&nbsp;GPU&nbsp;Computing&nbsp;Toolkit\CUDA\v11.8\bin<br><br>C:\Program&nbsp;Files\NVIDIA&nbsp;GPU&nbsp;Computing&nbsp;Toolkit\CUDA\v11.8\libnvvp<br></code></pre>
<ul data-tool="mdnice编辑器">
<li><section>nvcc版本和nvidia-smi 版本不一致：
虽然nvcc 是11.8版本，nvidia-smi显示的是12版本，并不冲突，因为：</section></li></ul>
<p data-tool="mdnice编辑器">nvcc 属于CUDA的编译器，将程序编译成可执行的二进制文件，nvidia-smi 全称是 NVIDIA System Management Interface ，是一种命令行工具，帮助管理和监控NVIDIA GPU设备的。</p>
<p data-tool="mdnice编辑器">nvcc是与CUDA Toolkit一起安装的CUDA compiler-driver tool，它只知道自身构建时的CUDA runtime版本，并不知道安装了什么版本的GPU driver。</p>
<h2 data-tool="mdnice编辑器"><span class="prefix"></span><span class="content">conda环境管理</span><span class="suffix"></span></h2>
<p data-tool="mdnice编辑器">Python项目环境依赖的版本让人很不好搞，对于不同cuda版本依赖，我们可以使用conda创建新环境，并安装对应的版本。
之前也写了一篇conda环境管理的文章。</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">conda&nbsp;install&nbsp;&nbsp;-c&nbsp;conda-forge&nbsp;cudatoolkit=11.8<br>conda&nbsp;install&nbsp;-c&nbsp;conda-forge&nbsp;cudnn<br></code></pre>
<p data-tool="mdnice编辑器">conda会自动给匹配合适的对应版本。</p>
<p data-tool="mdnice编辑器">哪怕本机部署Docker镜像，根据版本拉取hub中的镜像即可，例如：</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">docker&nbsp;pull&nbsp;pytorch/pytorch:1.13.1-cuda11.6-cudnn8-runtime<br></code></pre>
<h1 data-tool="mdnice编辑器"><span class="prefix"></span><span class="content">Mac 远程连接</span><span class="suffix"></span></h1>
<p data-tool="mdnice编辑器">Mac上可以远程<strong>ssh登录</strong>Win电脑，也可以使用<strong>相关软件</strong>，我用的Microsoft Remote Desktop，国内的APP store可能不可以下载，我这里放在了网盘，见文末。</p>
<p data-tool="mdnice编辑器">这样，主机打开，不需要额外显示器，而且可以Mac OS 和Windows系统无缝切换。</p>
<p data-tool="mdnice编辑器">1、安装 openssh
可以通过系统设置安装：</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">https://learn.microsoft.com/zh-cn/windows-server/administration/openssh/openssh_install_firstuse?<span class="hljs-built_in">source</span>=recommendations<br></code></pre>
<p data-tool="mdnice编辑器">也可以通过powerShell安装。</p>
<p data-tool="mdnice编辑器">Windows系统左下角的系统按钮，右键找到“命令提示符（管理员）”，全程需要管理员权限的哦。</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs"><span class="hljs-comment">#&nbsp;安装OpenSSH客户端</span><br>Add-WindowsCapability&nbsp;-Online&nbsp;-Name&nbsp;OpenSSH.Client~~~~0.0.1.0<br>&nbsp;<br><span class="hljs-comment">#安装OpenSSH服务端</span><br>Add-WindowsCapability&nbsp;-Online&nbsp;-Name&nbsp;OpenSSH.Server~~~~0.0.1.0<br></code></pre>
<p data-tool="mdnice编辑器">验证是否安装成功：
输入</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">Get-WindowsCapability&nbsp;-Online&nbsp;|&nbsp;?&nbsp;Name&nbsp;-like&nbsp;<span class="hljs-string">'OpenSSH*'</span><br></code></pre>
<p data-tool="mdnice编辑器">返回installed状态即可：
<img src="https://files.mdnice.com/user/38578/fb556138-269a-4106-b0bf-05779d259890.png" alt=""></p>
<p data-tool="mdnice编辑器">2、启动SSH服务器</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs"><span class="hljs-comment">#&nbsp;启动sshd服务</span><br>Start-Service&nbsp;sshd<br>&nbsp;<br><span class="hljs-comment">#&nbsp;将sshd服务设置为自动启动，若不设置需要在每次重启后重新开启sshd</span><br>Set-Service&nbsp;-Name&nbsp;sshd&nbsp;-StartupType&nbsp;<span class="hljs-string">'Automatic'</span><br>&nbsp;<br><span class="hljs-comment">#&nbsp;确认防火墙规则，一般在安装时会配置好</span><br>Get-NetFirewallRule&nbsp;-Name&nbsp;*ssh*<br>&nbsp;<br><span class="hljs-comment">#&nbsp;若安装时未添加防火墙规则"OpenSSH-Server-In-TCP"，则通过以下命令添加</span><br>New-NetFirewallRule&nbsp;-Name&nbsp;sshd&nbsp;-DisplayName&nbsp;<span class="hljs-string">'OpenSSH&nbsp;Server&nbsp;(sshd)'</span>&nbsp;-Enabled&nbsp;True&nbsp;-Direction&nbsp;Inbound&nbsp;-Protocol&nbsp;TCP&nbsp;-Action&nbsp;Allow&nbsp;-LocalPort&nbsp;22<br></code></pre>
<p data-tool="mdnice编辑器">3、开启密钥登录</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">ssh-keygen&nbsp;-t&nbsp;ed25519<br></code></pre>
<p data-tool="mdnice编辑器">如果走默认就一直回车，也可以自己定义路径和文件名，默认在：
C:\Users\你的用户名.ssh\id_ed25519
对应的还有个公钥 id_ed25519.pub</p>
<p data-tool="mdnice编辑器">我的这里加了个后缀，和其他文件区分开来。
<img src="https://files.mdnice.com/user/38578/d6e419b0-737d-46a5-b111-4f4617445a6b.png" alt=""></p>
<p data-tool="mdnice编辑器">我们将公钥.pub文件复制放到Mac系统下 .ssh文件下
<img src="https://files.mdnice.com/user/38578/7e3786f6-5f8b-4532-a34c-a79c16c07439.png" alt=""></p>
<ul data-tool="mdnice编辑器">
<li><section>SSH登录：</section></li></ul>
<p data-tool="mdnice编辑器">在Windows电脑的命令行中输入 ipconfig 查看 ipv4地址
在Mac终端输入：</p>
<pre class="custom" data-tool="mdnice编辑器"><code class="hljs">ssh&nbsp;win用户名@刚刚查看的ip地址<br></code></pre>
<p data-tool="mdnice编辑器"><img src="https://files.mdnice.com/user/38578/5841674f-56ac-4e3f-bbd2-981e99b9a119.png" alt="">
在终端操作需要熟悉Windows中的一些命令。</p>
<ul data-tool="mdnice编辑器">
<li><section>APP登录： Microsoft Remote Desktop</section></li></ul>
<p data-tool="mdnice编辑器">软件和本次的驱动、cuda我都上传到网盘了。从网盘下载安装即可。</p>
<p data-tool="mdnice编辑器">登录界面：
<img src="https://files.mdnice.com/user/38578/bdc40d4a-8331-4abf-a0e5-a6764c32b874.png" alt=""></p>
<p data-tool="mdnice编辑器">配置也简单：
在“add user account”中添加Windows主机的登录账密，保存下次就直接点击链接了。</p>
<figure data-tool="mdnice编辑器"><img src="https://files.mdnice.com/user/38578/383e1677-7b6b-40f4-bbc3-827ba53d2b9e.png" alt=""></figure>
<h2 data-tool="mdnice编辑器"><span class="prefix"></span><span class="content">其他连接方式：</span><span class="suffix"></span></h2>
<p data-tool="mdnice编辑器">如开头显示，我用了投影仪显示主机内容，一个投影仪和一个显示器也差不多的价格，但投影仪联网还能看视频看电影（家庭影院），屏幕和电视都是固定大小的屏幕。</p>
<p data-tool="mdnice编辑器">一根HDMI线，一头连接投影仪，一头连接显卡上的HDMI口就可以了，投影界面的设置中，将信号源切换为HDMI即可。</p>
<p data-tool="mdnice编辑器"><strong>END</strong></p>
<p data-tool="mdnice编辑器">本次分享就到这里了，Windows环境下基于RTX4090显卡安装深度学习环境的基本流程，Mac远程连接Windows主机、投影仪连接主机的相关图文教程。</p>

所有文件的百度云盘链接：
<a href="https://pan.baidu.com/s/17dqV0vD-Il3zxmKZDk4zcg?pwd=zcrn">https://pan.baidu.com/s/17dqV0vD-Il3zxmKZDk4zcg?pwd=zcrn </a></p>
<p data-tool="mdnice编辑器">需要的自取哈~</p>
<section class="footnotes-sep" data-tool="mdnice编辑器"></section>
<section class="footnotes" data-tool="mdnice编辑器">
<span id="fn1" class="footnote-item"><span class="footnote-num">[1] </span><p>知乎@良睦路程序员: <em>https://www.zhihu.com/question/586361676/answer/2913703371</em></p>
</span>
<span id="fn2" class="footnote-item"><span class="footnote-num">[2] </span><p>显卡驱动: <em>https://www.nvidia.cn/geforce/drivers/</em></p>
</span>
<span id="fn3" class="footnote-item"><span class="footnote-num">[3] </span><p>cudatoolkit: <em>https://developer.nvidia.com/cuda-toolkit-archive</em></p>
</span>
<span id="fn4" class="footnote-item"><span class="footnote-num">[4] </span><p>cuDNN: <em>https://developer.nvidia.com/rdp/cudnn-download</em></p>
</span>
</section>
</section>