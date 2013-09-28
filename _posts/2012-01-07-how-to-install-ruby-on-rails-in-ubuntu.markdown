---
layout: post
title: Ubuntu 11.10安装配置Ruby on Rails开发环境
---
1.初始系统安装开发包
{% highlight bash %}
    $ sudo apt-get install build-essential
{% endhighlight %}
2.安装RVM
{% highlight bash %}
    $ bash < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)
{% endhighlight %}
测试查看是否已经安装成功:
{% highlight bash %}
    $ rvm -v
    rvm 1.10.1 by Wayne E. Seguin <wayneeseguin@gmail.com>, Michal Papis <mpapis@gmail.com> [https://rvm.beginrescueend.com/]
{% endhighlight %}
修改用户bash相应配置文件~/.bashrc，确保每次用户登录时RVM脚本都会载入
{% highlight bash %}
    $ vim ~/.bashrc
    [[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" #增加此行
    $ source ~/.bashrc  
{% endhighlight %}
3.借助RVM安装Ruby环境
{% highlight bash %}
    $ rvm notes
{% endhighlight %}
可以列出标准版的Ruby所需的依赖包：
{% highlight bash %}
    $ sudo apt-get install build-essential bison openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev
{% endhighlight %}
安装ruby和rubygems：
{% highlight bash %}
    $ rvm list known  #列出哪些版本的Ruby可安装
    $ rvm install 1.9.2
    $ rvm --default use 1.9.2
{% endhighlight %}
检查确认下Ruby和RubyGems的版本：
{% highlight bash %}
    $ ruby -v
    $ gem -v
{% endhighlight %}
4.安装Rails环境
Rails 全部都打包在 Rails Gem 中。安装它是这个教程中最容易的部分。使用 RubyGems 来安装它，即 gem 命令。安装完成后，检查其版本来确保正确安装。
{% highlight bash %}
    $ sudo gem install bundler rails
    $ rails -v 
{% endhighlight %}
    注:必要时需修改下系统环境变量PATH：export /usr/local/bin/ruby:$PATH

