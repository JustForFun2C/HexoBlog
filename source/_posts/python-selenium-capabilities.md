---
title: 如何使用 selenium 改变文件下载路径
tags: [Selenium]
categories: Python
---

### 简介

- 可通过修改 webdriver.ChromeOptions() 来实现，可参考 [capabilities](https://sites.google.com/a/chromium.org/chromedriver/capabilities)

<!-- more -->

### 相关代码可参考

```python
chromeOptions = webdriver.ChromeOptions() 
prefs = {"download.default_directory" : "/some/path"} 
chromeOptions.add_experimental_option("prefs",prefs) 
chromedriver = "path/to/chromedriver.exe" 
driver = webdriver.Chrome(executable_path=chromedriver, chrome_options=chromeOptions) 
```