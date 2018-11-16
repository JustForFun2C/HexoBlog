---
title: 性能报告展示平台
tags: Project
categories: Project
---
### 项目背景
- WebPageTest 上测试结果并不能提供一个很直观的报告，而且只有当前这次的结果
- 需求有点诡异，大概有
	- 可以展示多次报告结果
	- 有图表展示
	- 图表结果自动发送邮件

<!-- more -->

### 方案
- 用 Jenkins 触发 api 在 WebPageTest 跑脚本，并获取报告地址
- 搭建一个性能报告展示平台，可以抓取 WebPageTest 报告，并进行图表展示
- 用 Python 写个小工具截图，并利用 Jenkins email 插件来插入图片发送邮件

### 使用框架
- Jenkins + Python + Flask + pyecharts
	- Jenkins 用来触发在 WebPageTest 跑脚本
	- Flask 用来快速搭建平台提供 api 
	- pyecharts 用来提供图表展示，可参考 [pyecharts-app][https://github.com/pyecharts/pyecharts-app] 

### 流程
- Jenkins 触发 WebPageTest api,并获取报告 url
- Jenkins 触发性能报告展示平台 api 抓取报告数据，并存入数据库
- 平台实时获取数据库数据进行图表展示
- Jenkins 是使用 Python 脚本截取相应图表图片
- Jenkins email 插件可在截图结束后，发送自定义 html 报告，将图片插入报告，并发送邮件

### 遇到的问题
- 并不是每一次触发 WebPageTest api 都能立刻开始跑脚本，有可能会遇到排队的情况，所以可能随后触发的抓起数据 api 抓不到任何数据
	- 解决方法：若没有抓取到数据，则每30秒重新抓取一次数据，2分钟后依旧没有抓取到就将各项数据置为 0 ，并保存报告地址，方便下一次抓取

- 会遇到上次未抓取数据，数据为0， 然后表格在做运算时就会有 0 除的 exception
	- 解决方法：每次访问表格页面时都重新抓取一次数据，确保不会有上一次数据为 0 的情况

- 关于截图，原先是采用 phantomjs 来进行全屏截取，不知道啥原因，表格一直渲染不出数据，而 selenium 自带的截图只能截取当前屏幕
	- 解决方法： 使用 chrome headless 无头模式
```Python
def headless_shotscreen(url, flag):
    capabilities = {
        'browserName': 'chrome',
        'chromeOptions': {
            'useAutomationExtension': False,
            'forceDevToolsScreenshot': True,
            'args': ['--start-maximized', '--disable-gpu', '--headless'],
        }
    }

    driver = webdriver.Chrome(desired_capabilities=capabilities)
    driver.set_page_load_timeout(300)
    driver.set_script_timeout(300)
    driver.get(url)

    # Get the actual page dimensions using javascript
    width = driver.execute_script(
        "return Math.max(document.body.scrollWidth, document.documentElement.clientWidth, document.documentElement.scrollWidth, document.documentElement.offsetWidth);")
    print(width)
    height = driver.execute_script(
        "return Math.max(document.body.scrollHeight, document.documentElement.clientHeight, document.documentElement.scrollHeight, document.documentElement.offsetHeight);")

    print(height)
    # resize
    driver.set_window_size(width, height)
    time.sleep(3)

    image = screen_path() + '/' + flag + '.png'
    driver.get_screenshot_as_file(image)
    driver.quit()
    return image
```

- Jenkins email 插件发送邮件不能在正文插入图片
	- 解决方法：在 Html 中 img 标签应使用 cid 属性, cid 后接的是附件的名称
```java
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>${ENV, var="JOB_NAME"}-第${BUILD_NUMBER}次构建日志</title>
<style> 
   img{
	width: auto;
	height: auto;
	max-width: 100%;
	max-height: 100%;	
}       
</sytle>
</head>
<body leftmargin="8" marginwidth="0" topmargin="8" marginheight="4"
    offset="0">
    <table width="95%" cellpadding="0" cellspacing="0"
        style="font-size: 11pt; font-family: Tahoma, Arial, Helvetica, sans-serif">
        <tr>
            <td><b><font color="#0B610B">Result:</font></b>
            <hr size="2" width="100%" align="center" /></td>
        </tr>
        <tr>
            <td>
                <ul>
                    <li>BUILD_NUMBER : ${BUILD_NUMBER}</li>
                    <li>BUILD_URL : <a href="${BUILD_URL}console">${BUILD_URL}console</a></li> 
                    <li>Check this link for the test results of current build : ${resultlink}</li>
                    <li>Report Center : http://sjtstsel13:3000</li>
                </ul> 
                
            </td>
        </tr>
    </table>
  <div>
      <img src="cid:result2.png" alt = 'result' />
 </div>
 <div>
     <img src="cid:line2.png" alt = 'line' />
 </div>
</body>
</html>
```
