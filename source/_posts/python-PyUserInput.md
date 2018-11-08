---
title: Python 模拟鼠标键盘操作
tags: [Python]
categories: Python
---

### 简介

- 可使用 [PyUserInput](https://pypi.org/project/PyUserInput/) 来实现，以下参照[Python-模拟鼠标键盘动作](https://www.jianshu.com/p/552f96aa85dc)

<!-- more -->

### 安装

- 依赖包
	- pywin32
	```python
	python -m pip install pypiwin32
	```
	- pyhook: 下载相应 python 版本的 [pyhook.whl](https://www.lfd.uci.edu/~gohlke/pythonlibs/#pyhook)
		- 安装
		```python
		pip install pyHook-1.5.1-cp36-cp36m-win_amd64.whl
		```

- 安装 PyUserInput
```python
pip install PyUserInput
```

### 调用方法
- 建立一个鼠标和键盘对象
```python
from pymouse import PyMouse
from pykeyboard import PyKeyboard
m = PyMouse()
k = PyKeyboard()
```

- 输入字符串
```python
k.type_string('Hello World')
```

- 发送键盘键入
```python
# pressing a key
k.press_key('H')

# which you then follow with a release of the key
k.release_key('H')

# or you can 'tap' a key which does both
k.tap_key('e')

# note that that tap_key does support a way of     repeating keystrokes with a interval time between each
k.tap_key('l',n=2,interval=5) 

# 支持特殊键
# Create an Alt+Tab combo
k.press_key(k.alt_key)
k.release_key(k.alt_key)

k.tap_key(k.tab_key)

# Tap F5
k.tap_key(k.function_keys[5]) 

# Tap 'Home' on the numpad
k.tap_key(k.numpad_keys['Home'])

 # Tap 5 on the numpad, thrice
k.tap_key(k.numpad_keys[5], n=3) # Tap 5 on the numpad, thrice

```

- windown 下组合键， 如 ctrl + shift + i
```python
k = PyKeyboard()

time.sleep(random.randint(2, 4))
k.press_key(k.control_key)
k.press_key(k.shift_key)
time.sleep(0.2)
k.tap_key('i')
k.release_key(k.shift_key)
k.release_key(k.control_key)
```