---
title: 批量遍历 52 states flows
tags: Project
categories: Project
---
### 项目背景

- 最近在做某产品线的部分回归脚本，大体的需求是
	- 通过输入一些对应的 zipcode / user info 来获取对应的 plans不同（quote page）
	- 判断 52 states 跳转的 quote page 是否是正确的

<!-- more -->

### 脚本使用框架
- Python + selenium

### 难点
- 如何判断不同的 quote page
- 单个用例流程有三个页面，但是遍历后52 states ，每个 state 有4种情况，总共有 200 多个 cases ，如何保证中途 driver 的稳定性
- 由第 2 点联想到，200多个 cases，如果顺利，需要至少 1 个半小时，如何缩短运行时间
- cases 中途失败，如何继续往下跑，并在最后进行重试
- cases 失败后如何查看失败情况
- 项目部署

### 脚本基本流程
- 启动 driver
- delete cookies
- 主页
- 填写信息页 (zipcode / user info)
- 判断 quote page

### 脚本分层
- init.py / init_db.py
	- 初始化
- configure.conf
	- 配置文件：数据库 / 测试domain / user info
- logic.py
	- 逻辑层：用来判断是哪种类型的 quote page
- db.py
	- db 相关操作
- tools.py
	- 方法封装：如获取报告路径 / 获取不同 env 对应的 domian 等等
- user_flows.py
	- 使用 Webdriver 走 UI 到达 quote page 的 flows
- generate_finally_report.py
	- 读取数据库生成报告
- XXX-test.py
	- 脚本入口：包括处理 exception 和重跑机制 
	- 重跑机制：跑完脚本后去数据库搜索 result 为 no 的结果，获取 zipcode 相关信息重跑，然后更新数据库数据

### 经验总结
- 对脚本一定要进行分层，比如单独把判断 quote page 的逻辑封装，在验证完逻辑没问题后，后面为了维护脚本稳定性就不会动到逻辑这块，不需要重新大规模验证

- 大量运行的用例会使 remote driver 不太稳定，经常出现 driver connect time out 的情况
	- 解决方法：每 4 个用例过后就重启一次 driver,在 4 次用例中如再有失败，则记录下相应的信息并重启 driver ，在脚本循环结束后进行重跑,并且每一次到达 quote page 都进行截图，方便后面排查问题
	```Python
	while True:
    	if state_count != 0:
        	driver = start_new_driver()
        	for i in range(4):
        		try:
        			# to do
        		exception:
        			# write the releted info to the error file for rerun the cases last
        			result = ('no', 'url')
        	# restart the driver			
        	if 'no' in result[0]:
                driver = start_new_driver()
            else
                # screen shot
	```
	
- python 本身没有并发机制，在 Jenkins 上部署多个 Job 来进行伪并发，但是要解决报告数据合并的问题
	- 解决方法：将每个 job 结束后报告数据存入数据库，等所有 job 完成后，再从数据库重新取出，生成最后的报告
