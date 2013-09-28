---
layout: post
title: Nagios版本升级
---
总体步骤：
1.下载最新安装包：nagios-3.4.1.tar.gz
2.解压编译：
{% highlight bash %}
    tar zxf nagios-3.4.1.tar.gz
    cd nagios
    ./configure --with-command-group=nagcmd
    make all
    make install
{% endhighlight %}
3.测试验证：
{% highlight bash %}
    /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
{% endhighlight %}
4.重启服务：
{% highlight bash %}
    service nagios restart
{% endhighlight %}
