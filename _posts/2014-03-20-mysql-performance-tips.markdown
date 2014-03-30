---
layout: post
title: MySQL Performance Tips 
---

#### 1. Never trust anyone,benchmark

    test–>benchmark–>monitor

    sysbench/mysqlslap,monitor tools

#### 2.Make sure you have enough RAM

	Depands on your active data and connections

	1.active data should fit in the buffer pool
	2.connections and caches take memory

#### 3.Use fast and multi-core processors

#### 4.Use fast and reliable storage

	1.SSD
	2.SATA for log files
	3.several disks (raid5 raid10)

#### 5.Choose the right OS

#### 6.Adjust the OS limits

	1.Max open files per process:ulimit -n      
		limits the number of file handles (connections,open tables,…)
	2.Max threads per user:ulimit -u
		limits the number of threads

#### 7.Consider using alternative malloc

	1.jemalloc
	2.tcmalloc

#### 8.Choose the right file system

	1.XFS:excellent,innode_flush_method=O_DIRECT,less stable recently
	2.EXT4:best choice for speed and ease of use,fsyncs a bit slower than ext3,more reliable
	3.EXT3

#### 9.Mount options

	1.EXT4(rw,noatime,nodiratime,nobarrier,data=ordered)
	2.XFS(rw,noatime,nodiratime,nobarrier,logbufs=8,logbsize=32k)

	SSD specific:innodb_page_size=4k,innodb_flush_neighbors=0

#### 10.Choose the best I/O scheduler

	1.deadline:generall the best I/O scheduler
		echo deadline > /sys/block/{DEVICE-NAME}/queue/scheduler
	2.noop:the best value is HW and WL specific (SSD,good RAID card…)

#### 11.Use a battery backed disk cache

	1.faster fsyncs:innodb redo logs / binary logs / data files
	2.Crash safety

#### 12.Balance the load on several disks

#### 13.Use the Thread Pool

#### 14.Configure table caching
	1.table_open_cache:used to size PS,opened_tables/sec
	2.table_definition_cache:increase
	3.table_cache_instances:8 or 16
	4.innodb_open_files
	5.mdl_hash_instances=256

#### 15.Cache the threads:Thread creation / initalization is expensive

	thread_cache_size:decreases threads_created rate

#### 16.Reduce per thread memory usage

	max_used_connections*( 
		read_buffer_size +
		read_rnd_buffer_size +
		join_buffer_size +
		sort_buffer_size +
		binlog_cache_size +
		thread_stack +
		2 * net_buffer_length …
	)

#### 17.Beware of sync\_binlog=1

	set sync_binlog=0

#### 18.Use InnoDB Engine

#### 19.Use a large buffer pool

	innodb_buffer_pool_size:do not swap,beware of memory crash
		eg:Active dat <= innodb_buffer_pool_size <= 80% RAM

#### 20.Reduce the buffer pool contention

	1.innodb_buffer_pool_instances >=8
	2.reduce rows_examined / sec

#### 21.Use largge redo logs

	a key parameter for write performance:better for write QPS

#### 22.Adjust the IO capactiy

#### 23.Configure the InnoDB flushing

	1.Redo logs:innodb_flush_log_at_trx_commit
			=1 best durability
			=2 better performance
			=0 best performance
		advice set = 2
	2.Data files only:innodb_flush_method

	O_DIRECT:for Linux,skips the FS cache

#### 24.Enable innodb\_file\_per\_table

	1.increased manageability
	2.truncate reclaims disk space
	3.better with innodb\_flush\_method=O\_DIRECT
	4.easier to optimize

  But:

	1.not so good with many small tables;
	2.more file handles
	3.more fsyncs

#### 25.Configure the thread concurrency 

	1.No thread pool:
		innodb_thread_concurrency=16~32 in 5.5
		innodb_thread_concurrency=36 in 5.6
	2.Thread pool:
		innodb_thread_concurrency=0
		innodb_max_concurrency_tickets

#### 26.Reduce the transaction isolation
#### 27.Design the tables
#### 28.Add or Remove unused indexes
#### 29.Reduce rows\_examined /row\_sent / locking,Mine the slow query log

