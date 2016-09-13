---
title: PHP多进程实践
date: 2016-09-13 16:39:04
tags:
categories:
---

利用pcntl_fork实现PHP多进程代码如下:

```
function runTasks($tasks)
	{
		$linenum = 2;
		$count = count($tasks);
		if ($count > 30) {
			$linenum = (int)($count/15);
		}
		//任务分割
		$chunks = array_chunk($tasks, $linenum);
		$procs = array();
		foreach ($chunks as $chunk) {
			$pid = pcntl_fork();
			if ($pid == -1) {
				die('could not fork');
			} else if ($pid) {
				$procs[] = $pid;
			} else {
				foreach ($chunk as $line) {
					system('php '. $line);
					echo $line." done!\n";
				}
				exit(0);
			}
		}
		foreach ($procs as $proc) {
			pcntl_waitpid($proc, $status);
		}
		unset($pid);
		$pid = NULL;
		unset($procs);
		$procs = NULL;
		unset($chunks);
		$chunks = NULL;
	}
```

部署脚本：

```
#!/bin/bash
source /etc/profile
source /root/.bash_profile

nowDate=`date +%Y%m%d`
cd /yourpath

nowTime=`date +%M`
//每小时一次 01分的时候
if [ "$nowTime" -eq "01" ]
then
	ProcessCount=`ps -ef|grep "tash.php" |grep -v "grep"|wc -l`
	if [ $ProcessCount -lt 1 ]
		then
		nohup /usr/bin/php test.php >> /dev/null &
		echo "Create test.php run!"
	fi
fi
```

