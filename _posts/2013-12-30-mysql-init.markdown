---
layout: post
title: 关于MySQL环境平台配置初始化
---

### 一、Raid卡使用

raid1 (2 块盘)， 用于系统及mysql binlog 

raid10 (4 块SSD 盘)，用于mysql data

raid缓存设置： 

1. 读:no ahead
2. 写: writethrough #writeback 写 cache 据说之前出现过系统诡异异常崩溃，暂未验证保守保持使用through
3. 条带: 默认 64k #没有确认的测试经验

{% highlight bash %}
MegaCli64 -LDGetProp -Cache -LALL -a0
Adapter 0-VD 0(target id: 0): Cache Policy:WriteThrough, ReadAhead, Direct, No Write Cache if bad BBU
Adapter 0-VD 1(target id: 1): Cache Policy:WriteThrough, ReadAhead, Direct, No Write Cache if bad BBU
{% endhighlight %}

### 二、内存方面

1. BIOS直接设置关闭numa
2. 关闭swap
{% highlight bash %}
System BIOS – Memory Settings – Node Interleaving – enabled

vm.swappiness = 0 
{% endhighlight %}

### 三、文件系统

{% highlight bash %}
echo 16 > /sys/block/sdb/queue/read_ahead_kb #减少预读
echo 512 > /sys/block/sdb/queue/nr_requests #增加队列
{% endhighlight %}

b)IO调度策略修改为deadline。

{% highlight bash %}
echo deadline >/sys/block/sdb/queue/scheduler
{% endhighlight %}

c)用noatime，nobarrier挂载磁盘

{% highlight bash %}
mount -L /usr/local/mysql -o defaults,noatime,barrier=0 /usr/local/mysql
mount -L /usr/mysqldata -o defaults,noatime,barrier=0 /usr/mysqldata
{% endhighlight %}

### 四、网络

双网卡绑定

### 五、关闭系统默认文件数/线程数最大打开数限制设置

{% highlight bash %}
ulimit -SHn 65535
ulimit -f unlimited
ulimit -u unlimited
{% endhighlight %}

### 六、MySQL配置文件

比如：
{% highlight bash %}
#Change connection & timeout 
max_connections
max_user_connections
interactive_timeout
wait_timeout
#Setting binlog save 10 days and sync mode
expire_logs_days=9
sync-binlog=0
#Use per table data & Change commit and flush mode
innodb_file_per_table = 1
innodb_flush_log_at_trx_commit = 2
innodb_flush_method = O_DIRECT
{% endhighlight %}
