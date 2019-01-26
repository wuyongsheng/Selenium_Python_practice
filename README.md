
>  自2016年开始接触 web 自动化测试工具 selenium 以来，在网上查阅了很多相关的资料，购买了 selenium 的书籍和视频，在工作中也使用过 selenium ，目前已经能够熟练地使用 Python 及 Java 语言操作 selenium了。这篇笔记用来对 selenium 的一些基本操作进行一个总结。

###  安装selenium3
使用 selenium 之前，需要准备好相应的环境：安装 Python（我使用的是Python 3.7），安装 Python 的编辑器（我使用的是eclipse），安装Python 的 selenium 库（可以通过 pip 安装：pip3 install selenium），因为 selenium 是驱动浏览器进行自动化测试的，所有还要准备好浏览器及其驱动（我使用的浏览器是Chrome，驱动是Chromedriver），同时浏览器驱动的版本要和浏览器的版本对应。

这些网上很容易就能查的到，不做详述。

基本的环境都准备好了之后，可以通过下面的代码进行测试：
```
from selenium import webdriver


driver = webdriver.Chrome()
driver.get('http://www.wysh.site')

print(driver.title)

driver.quit()
```
上面的代码是用来打印网站的标题，，如果能够成功打印网站的标题，说明环境没有问题，运行结果如下图：

![title.jpg](https://upload-images.jianshu.io/upload_images/12273007-ff9dca61971bf04e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###  selenium3 浏览器驱动

下载浏览器驱动
当selenium升级到3.0之后，对不同的浏览器驱动进行了规范。如果想使用selenium驱动不同的浏览器，必须单独下载并设置不同的浏览器驱动。

我使用的是Chrome浏览器及其驱动

Chrome浏览器驱动：chromedriver , [taobao地址](https://npm.taobao.org/mirrors/chromedriver)

我们要下载一个和 Chrome 浏览器版本对应的chromedriver，Chrome浏览器版本和 chromedriver 版本的对应关系可以自行百度。

接着，将下载好的 chromedriver.exe 放到  Chrome 浏览器的安装目录（chrome.exe 所在的目录）以及 Python 的安装目录（python.exe 所在的目录）。

最后，我们还要配置环境变量，将Chrome 浏览器的安装目录的路径添加到 Path 环境变量中。

我的电脑-->属性-->系统设置-->高级-->环境变量-->系统变量-->Path，将Chrome 浏览器的安装目录的路径添加到 Path 环境变量中。

- Path
- ;C:\Users\Administrator\AppData\Local\Google\Chrome\Application

###  selenium元素定位

Selenium提供了8种定位方式。

- id
- name
- class name
- tag name
- link text
- partial link text
- xpath
- css selector

这8种定位方式在Python selenium中所对应的方法为：

- find_element_by_id()
- find_element_by_name()
- find_element_by_class_name()
- find_element_by_tag_name()
- find_element_by_link_text()
- find_element_by_partial_link_text()
- find_element_by_xpath()
- find_element_by_css_selector()

**定位方法的用法**

假如我们有一个Web页面，通过前端工具（如，Firebug）查看到一个元素的属性是这样的。
```
<html>
  <head>
  <body link="#0000cc">
    <a id="result_logo" href="/" onmousedown="return c({'fm':'tab','tab':'logo'})">
    <form id="form" class="fm" name="f" action="/s">
      <span class="soutu-btn"></span>
        <input id="kw" class="s_ipt" name="wd" value="" maxlength="255" autocomplete="off">
```
我们的目的是要定位input标签的输入框。

- 通过id定位:
```
dr.find_element_by_id("kw")
```
- 通过name定位:
```
dr.find_element_by_name("wd")
```
- 通过class name定位:
```
dr.find_element_by_class_name("s_ipt")
```
- 通过tag name定位:
```
dr.find_element_by_tag_name("input")
```
-  通过xpath定位，xpath定位有多种写法，这里列几个常用写法:
```
dr.find_element_by_xpath("//*[@id='kw']")
dr.find_element_by_xpath("//*[@name='wd']")
dr.find_element_by_xpath("//input[@class='s_ipt']")
dr.find_element_by_xpath("/html/body/form/span/input")
dr.find_element_by_xpath("//span[@class='soutu-btn']/input")
dr.find_element_by_xpath("//form[@id='form']/span/input")
dr.find_element_by_xpath("//input[@id='kw' and @name='wd']")
```
-  通过css定位，css定位有多种写法，这里列几个常用写法:
```
dr.find_element_by_css_selector("#kw")
dr.find_element_by_css_selector("[name=wd]")
dr.find_element_by_css_selector(".s_ipt")
dr.find_element_by_css_selector("html > body > form > span > input")
dr.find_element_by_css_selector("span.soutu-btn> input#kw")
dr.find_element_by_css_selector("form#form > span > input")
```
接下来，我们的页面上有一组文本链接。
```
<a class="mnav" href="http://news.baidu.com" name="tj_trnews">新闻</a>
<a class="mnav" href="http://www.hao123.com" name="tj_trhao123">hao123</a>
```
- 通过link text定位:
```
dr.find_element_by_link_text("新闻")
dr.find_element_by_link_text("hao123")
```
- 通过link text定位:
```
dr.find_element_by_partial_link_text("新")
dr.find_element_by_partial_link_text("hao")
dr.find_element_by_partial_link_text("123")
```
- 关于xpaht和css的定位比较复杂，可以参考 [w3school](http://www.w3school.com.cn/)

###  控制浏览器操作

- 控制浏览器窗口大小

有时候我们希望能以某种浏览器尺寸打开，让访问的页面在这种尺寸下运行。例如可以将浏览器设置成移动端大小(480* 800)，然后访问移动站点，对其样式进行评估；WebDriver提供了set_window_size()方法来设置浏览器的大小。
```
from selenium import webdriver

driver = webdriver.Chrome()
driver.get("http://m.baidu.com")

# 参数数字为像素点
print("设置浏览器宽480、高800显示")
driver.set_window_size(480, 800)
driver.quit()
```
在PC端执行自动化测试脚本大多的情况下是希望浏览器在全屏幕模式下执行，那么可以使用maximize_window()方法使打开的浏览器全屏显示，其用法与set_window_size() 相同，但它不需要参数。



控制浏览器后退、前进
在使用浏览器浏览网页时，浏览器提供了后退和前进按钮，可以方便地在浏览过的网页之间切换，WebDriver也提供了对应的back()和forward()方法来模拟后退和前进按钮。下面通过例子来演示这两个方法的使用。
```
from selenium import webdriver
from time import sleep

driver = webdriver.Chrome()

#访问百度首页
first_url= 'http://www.baidu.com'
print("now access %s" %(first_url))
driver.get(first_url)
sleep(2)

#访问新闻页面
second_url='http://news.baidu.com'
print("now access %s" %(second_url))
driver.get(second_url)
sleep(2)

#返回（后退）到百度首页
print("back to  %s "%(first_url))
driver.back()
sleep(2)

#前进到新闻页
print("forward to  %s"%(second_url))
driver.forward()

driver.quit()
```
为了看清脚本的执行过程，下面每操作一步都通过print()来打印当前的URL地址。

刷新页面
有时候需要手动刷新（F5） 页面。
```
driver.refresh() #刷新当前页面
```

###  WebDriver常用方法

#### 点击和输入

前面我们已经学习了定位元素， 定位只是第一步， 定位之后需要对这个元素进行操作， 或单击（按钮） 或输入（输入框） ， 下面就来认识 WebDriver 中最常用的几个方法：

- clear()： 清除文本。

- send_keys (value)： 模拟按键输入。

- click()： 单击元素。

```
from selenium import webdriver
import time

driver = webdriver.Chrome()
driver.get("https://www.baidu.com")

driver.find_element_by_id("kw").clear()
driver.find_element_by_id("kw").send_keys("selenium")
driver.find_element_by_id("su").click()
time.sleep(5)

driver.quit()
```

#### 提交

- submit()

submit()方法用于提交表单。 例如， 在搜索框输入关键字之后的“回车” 操作， 就可以通过该方法模拟。
```
from selenium import webdriver
import time

driver = webdriver.Chrome()
driver.get("https://www.baidu.com")

driver.find_element_by_id("kw").clear()
driver.find_element_by_id("kw").send_keys("selenium")
driver.find_element_by_id("su").submit()
time.sleep(5)

driver.quit()
```
有时候 submit()可以与 click()方法互换来使用， submit()同样可以提交一个按钮， 但 submit()的应用范围远不及 click()广泛。



其他常用方法
- size： 返回元素的尺寸。

- text： 获取元素的文本。

- get_attribute(name)： 获得属性值。

- is_displayed()： 设置该元素是否用户可见。
```
from selenium import webdriver

driver = webdriver.Chrome()
driver.get("http://www.baidu.com")

# 获得输入框的尺寸
size = driver.find_element_by_id('kw').size
print(size)

# 返回百度页面底部备案信息
text = driver.find_element_by_id("cp").text
print(text)

driver.quit()
```
返回元素的属性值， 可以是 id、 name、 type 或其他任意属性
```
attribute = driver.find_element_by_id("kw").get_attribute('type')
print(attribute)
```
返回元素的结果是否可见， 返回结果为 True 或 False
```
result = driver.find_element_by_id("kw").is_displayed()
print(result)

driver.quit()
```
输出结果：
```
{'height': 22, 'width': 500}
©2019 Baidu 使用百度前必读 意见反馈 京ICP证030173号  京公网安备11000002000001号 
text
True
```
执行上面的程序并查看结果： size 方法用于获取百度输入框的宽、 高， text 方法用于获得百度底部的备案信息，

get_attribute()用于获得百度输入的 type 属性的值，

is_displayed()用于返回一个元素是否可见， 如果可见则返回 True， 否则返回 False。

###  鼠标事件

在 WebDriver 中， 将这些关于鼠标操作的方法封装在 ActionChains 类提供。

ActionChains 类提供了鼠标操作的常用方法：

- perform()： 执行所有 ActionChains 中存储的行为；

- context_click()： 右击；

- double_click()： 双击；

- drag_and_drop()： 拖动；

- move_to_element()： 鼠标悬停。



- **鼠标悬停操作**

```
from selenium import webdriver
import time
# 引入 ActionChains 类
from selenium.webdriver.common.action_chains import ActionChains

driver = webdriver.Chrome()
driver.get("https://www.baidu.cn")
time.sleep(3)
# 定位到要悬停的元素
above = driver.find_element_by_link_text("设置")
# 对定位到的元素执行鼠标悬停操作
ActionChains(driver).move_to_element(above).perform()
time.sleep(3)

driver.quit()

```
```
from selenium.webdriver import ActionChains
```
导入提供鼠标操作的 ActionChains 类。

ActionChains(driver)

调用 ActionChains()类， 将浏览器驱动 driver 作为参数传入。

move_to_element(above)

context_click()方法用于模拟鼠标右键操作， 在调用时需要指定元素定位。

perform()
执行所有 ActionChains 中存储的行为， 可以理解成是对整个操作的提交动作。 

### 键盘事件

Keys()类提供了键盘上几乎所有按键的方法。 前面了解到， send_keys()方法可以用来模拟键盘输入， 除此 之外， 我们还可以用它来输入键盘上的按键， 甚至是组合键， 如 Ctrl+A、 Ctrl+C 等。
```
from selenium import webdriver
import time
# 引入 Keys 模块
from selenium.webdriver.common.keys import Keys

driver = webdriver.Chrome()
driver.get("http://www.baidu.com")
time.sleep(2)
# 输入框输入内容
driver.find_element_by_id("kw").send_keys("seleniumm")
time.sleep(2)
# 删除多输入的一个 m
driver.find_element_by_id("kw").send_keys(Keys.BACK_SPACE)

time.sleep(2)
# 输入空格键+“教程”
driver.find_element_by_id("kw").send_keys(Keys.SPACE)
time.sleep(2)
driver.find_element_by_id("kw").send_keys("教程")
time.sleep(2)
# ctrl+a 全选输入框内容
driver.find_element_by_id("kw").send_keys(Keys.CONTROL, 'a')
time.sleep(2)
# ctrl+x 剪切输入框内容
driver.find_element_by_id("kw").send_keys(Keys.CONTROL, 'x')
time.sleep(2)
# ctrl+v 粘贴内容到输入框
driver.find_element_by_id("kw").send_keys(Keys.CONTROL, 'v')
time.sleep(2)
# 通过回车键来代替单击操作
driver.find_element_by_id("su").send_keys(Keys.ENTER)
time.sleep(2)
driver.quit()
```
需要说明的是， 上面的脚本没有什么实际意义， 仅向我们展示模拟键盘各种按键与组合键的用法。
```
from selenium.webdriver.common.keys import Keys
```
在使用键盘按键方法前需要先导入 keys 类。

以下为常用的键盘操作：

- send_keys(Keys.BACK_SPACE) 删除键（BackSpace）

- send_keys(Keys.SPACE) 空格键(Space)

- send_keys(Keys.TAB) 制表键(Tab)

- send_keys(Keys.ESCAPE) 回退键（Esc）

- send_keys(Keys.ENTER) 回车键（Enter）

- send_keys(Keys.CONTROL,'a') 全选（Ctrl+A）

- send_keys(Keys.CONTROL,'c') 复制（Ctrl+C）

- send_keys(Keys.CONTROL,'x') 剪切（Ctrl+X）

- send_keys(Keys.CONTROL,'v') 粘贴（Ctrl+V）

- send_keys(Keys.F1) 键盘 F1

- ……

- send_keys(Keys.F12) 键盘 F12

###  获取断言信息

不管是在做功能测试还是自动化测试，最后一步需要拿实际结果与预期进行比较。这个比较称之为断言。

我们通常可以通过获取title 、URL和 text 等信息进行断言。text 方法在前面已经讲过，它用于获取标签对之间的文本信息。 下面同样以百度为例，介绍如何获取这些信息。
```
from selenium import webdriver
from time import sleep

driver = webdriver.Chrome()
driver.get("https://www.baidu.com")

print('Before search================')

# 打印当前页面title
title = driver.title
print(title)

# 打印当前页面URL
now_url = driver.current_url
print(now_url)

driver.find_element_by_id("kw").send_keys("selenium")
driver.find_element_by_id("su").click()
sleep(1)

print('After search================')

# 再次打印当前页面title
title = driver.title
print(title)

# 打印当前页面URL
now_url = driver.current_url
print(now_url)

# 获取结果数目
user = driver.find_element_by_class_name('nums').text
print(user)

driver.quit()
```
脚本运行结果如下：
```
Before search================
百度一下，你就知道
https://www.baidu.com/
After search================
selenium_百度搜索
https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=0&rsv_idx=1&tn=baidu&wd=selenium&rsv_pq=db3e8f63000a6edc&rsv_t=7841E0UXWIO0Nol1kjIjMzDYAdvgaLaT0yVYxm7ixq1GrySrvsDw%2Be26Cx0&rqlang=cn&rsv_enter=0&rsv_sug3=8&rsv_sug1=2&rsv_sug7=100&inputT=477&rsv_sug4=478
搜索工具
百度为您找到相关结果约17,300,000个
```
- title：用于获得当前页面的标题。

- current_url：用户获得当前页面的URL。

- text：获取搜索条目的文本信息。

###  设置元素等待

WebDriver提供了两种类型的等待：显式等待和隐式等待。

####  显式等待
显式等待使WebdDriver等待某个条件成立时继续执行，否则在达到最大时长时抛出超时异常（TimeoutException）。
```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()
driver.get("http://www.baidu.com")

element = WebDriverWait(driver, 5, 0.5).until(
                      EC.presence_of_element_located((By.ID, "kw"))
                      )
element.send_keys('selenium')
driver.quit()
```
WebDriverWait类是由WebDirver 提供的等待方法。在设置时间内，默认每隔一段时间检测一次当前页面元素是否存在，如果超过设置时间检测不到则抛出异常。具体格式如下：
```
WebDriverWait(driver, timeout, poll_frequency=0.5, ignored_exceptions=None)
````
driver ：浏览器驱动。

timeout ：最长超时时间，默认以秒为单位。

poll_frequency ：检测的间隔（步长）时间，默认为0.5S。

ignored_exceptions ：超时后的异常信息，默认情况下抛NoSuchElementException异常。

WebDriverWait()一般由until()或until_not()方法配合使用，下面是until()和until_not()方法的说明。

until(method, message=‘’)
调用该方法提供的驱动程序作为一个参数，直到返回值为True。

until_not(method, message=‘’)
调用该方法提供的驱动程序作为一个参数，直到返回值为False。

在本例中，通过as关键字将expected_conditions 重命名为EC，并调用presence_of_element_located()方法判断元素是否存在。

####  隐式等待
WebDriver提供了implicitly_wait()方法来实现隐式等待，默认设置为0。它的用法相对来说要简单得多。
```
from selenium import webdriver
from selenium.common.exceptions import NoSuchElementException
from time import ctime

driver = webdriver.Chrome()

# 设置隐式等待为10秒
driver.implicitly_wait(10)
driver.get("http://www.baidu.com")

try:
    print(ctime())
    driver.find_element_by_id("kw22").send_keys('selenium')
except NoSuchElementException as e:
    print(e)
finally:
    print(ctime())
    driver.quit()
```
implicitly_wait() 默认参数的单位为秒，本例中设置等待时长为10秒。

首先这10秒并非一个固定的等待时间，它并不影响脚本的执行速度。其次，它并不针对页面上的某一元素进行等待。当脚本执行到某个元素定位时，如果元素可以定位，则继续执行；如果元素定位不到，则它将以轮询的方式不断地判断元素是否被定位到。假设在第6秒定位到了元素则继续执行，若直到超出设置时长（10秒）还没有定位到元素，则抛出异常。

### 定位一组元素

WebDriver还提供了8种用于定位一组元素的方法。
```
find_elements_by_id()
find_elements_by_name()
find_elements_by_class_name()
find_elements_by_tag_name()
find_elements_by_link_text()
find_elements_by_partial_link_text()
find_elements_by_xpath()
find_elements_by_css_selector()
```
定位一组元素的方法与定位单个元素的方法类似，唯一的区别是在单词element后面多了一个s表示复数。

接下来通过例子演示定位一组元素的使用：
```
from selenium import webdriver
from time import sleep

driver = webdriver.Chrome()
driver.get("https://www.baidu.com")

driver.find_element_by_id("kw").send_keys("wysh.site")
driver.find_element_by_id("su").click()
sleep(1)

# 定位一组元素
texts = driver.find_elements_by_xpath('//div/h3/a')
print(texts)
# 循环遍历出每一条搜索结果的标题
for t in texts:
    print(t.text)

driver.quit()
```
程序运行结果：
```
Vincent's Home
“wysh.site”的权重综合查询结果 - 站长工具
WYSH AM 1380 – Anderson County's Classic Hit Country!
Wyshmaster - Wikipedia
Wysh Data Systems Limited Hong Kong company address, contacts...
WyshmasterBeats.com
ORPD: 3 arrested after alleged robbery – WYSH AM 1380
neets
NHL Wysh List: NHL players deserve better than this
Games - Bigpoint
```

###  多表单切换

在Web应用中经常会遇到frame/iframe表单嵌套页面的应用，WebDriver只能在一个页面上对元素识别与定位，对于frame/iframe表单内嵌页面上的元素无法直接定位。这时就需要通过switch_to.frame()方法将当前定位的主体切换为frame/iframe表单的内嵌页面中。
```
<html>
  <body>
    ...
    <iframe id="x-URS-iframe1548501566204.4119" ...>
      <html>
         <body>
           ...
           <input name="email" >
```
![frame.jpg](https://upload-images.jianshu.io/upload_images/12273007-91541215f95b41b8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

126邮箱登录框的结构大概是这样子的，想要操作登录框必须要先切换到iframe表单。

可以查出，页面只提供了 frame 的 id ，并且 id 是变化的，这样就无法通过 id 进行定位了

对于变化的 id 的 frame 元素的定位，可以参考下面的链接：

https://blog.csdn.net/genius_man/article/details/80903291

我是先通过 xpath 找到该 frame 上一级，再在该 xpath 路径后面加上 /iframe ，就可以定位该 frame 了。

![iframe上一层元素.jpg](https://upload-images.jianshu.io/upload_images/12273007-e448b163c6394fc6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
from selenium import webdriver
from time import  sleep
driver = webdriver.Chrome()
driver.get("http://www.126.com")
sleep(2)
i=driver.find_element_by_xpath('//*[@id="loginDiv"]/iframe')
driver.switch_to.frame(i)
driver.find_element_by_name("email").clear()
driver.find_element_by_name("email").send_keys("username")
driver.find_element_by_name("password").clear()
driver.find_element_by_name("password").send_keys("password")
driver.find_element_by_id("dologin").click()
driver.switch_to.default_content()

driver.quit()
```
switch_to.frame() 默认可以直接取表单的id 或name属性。如果iframe没有可用的id和name属性，则可以通过下面的方式进行定位。
```
……
#先通过xpth定位到iframe
xf = driver.find_element_by_xpath('//*[@id="x-URS-iframe"]')

#再将定位对象传给switch_to.frame()方法
driver.switch_to.frame(xf)
……
driver.switch_to.parent_frame()
```
除此之外，在进入多级表单的情况下，还可以通过switch_to.default_content()跳回最外层的页面。

###  多窗口切换

在页面操作过程中有时候点击某个链接会弹出新的窗口，这时就需要主机切换到新打开的窗口上进行操作。WebDriver提供了switch_to.window()方法，可以实现在不同的窗口之间切换。 以百度首页和百度注册页为例，在两个窗口之间的切换如下图。
```
from selenium import webdriver
import time

driver = webdriver.Chrome()
driver.implicitly_wait(10)
driver.get("http://www.baidu.com")

# 获得百度搜索窗口句柄
sreach_windows = driver.current_window_handle

driver.find_element_by_link_text('登录').click()
driver.find_element_by_link_text("立即注册").click()

# 获得当前所有打开的窗口的句柄
all_handles = driver.window_handles

# 进入注册窗口
for handle in all_handles:
        print(handle)
        driver.switch_to.window(handle)
        if(driver.title=='注册百度帐号'):
           print('now register window!')
           driver.find_element_by_id("TANGRAM__PSP_3__userName").send_keys('username')
           driver.find_element_by_id("TANGRAM__PSP_3__phone").send_keys('18574848987')
           driver.find_element_by_id('TANGRAM__PSP_3__verifyCode').send_keys('7874')
           time.sleep(2)
driver.quit()
```
在本例中所涉及的新方法如下：

- current_window_handle：获得当前窗口句柄。

- window_handles：返回所有窗口的句柄到当前会话。

- switch_to.window()：用于切换到相应的窗口，与上一节的switch_to.frame()类似，前者用于不同窗口的切换，后者用于不同表单之间的切换。

###  警告框处理

在WebDriver中处理JavaScript所生成的alert、confirm以及prompt十分简单，具体做法是使用 switch_to.alert 方法定位到 alert/confirm/prompt，然后使用 text/accept/dismiss/ send_keys等方法进行操作。

- text：返回 alert/confirm/prompt 中的文字信息。

- accept()：接受现有警告框。

- dismiss()：解散现有警告框。

- send_keys(keysToSend)：

发送文本至警告框。keysToSend：将文本发送至警告框。

百度搜索设置弹出的窗口是不能通过前端工具对其进行定位的，这个时候就可以通过switch_to_alert()方法接受这个弹窗。 
```
from selenium import webdriver
from selenium.webdriver.common.action_chains import ActionChains
import time

driver = webdriver.Chrome()
driver.implicitly_wait(10)
driver.get('http://www.baidu.com')

# 鼠标悬停至“设置”链接
link = driver.find_element_by_link_text('设置')
ActionChains(driver).move_to_element(link).perform()
time.sleep(2)
# 打开搜索设置
driver.find_element_by_link_text("搜索设置").click()

# 保存设置
driver.find_element_by_class_name("prefpanelgo").click()
time.sleep(2)

# 接受警告框
driver.switch_to.alert.accept()

driver.quit()
```
通过switch_to_alert()方法获取当前页面上的警告框，并使用accept()方法接受警告框。

###  下拉框选择

有时我们会碰到下拉框，WebDriver提供了Select类来处理下拉框。 如百度搜索设置的下拉框，如下图：
```
from selenium import webdriver
from selenium.webdriver.support.select import Select
from time import sleep

driver = webdriver.Chrome()
driver.implicitly_wait(10)
driver.get('http://www.baidu.com')

# 鼠标悬停至“设置”链接
driver.find_element_by_link_text('设置').click()
sleep(1)
# 打开搜索设置
driver.find_element_by_link_text("搜索设置").click()
sleep(2)

# 搜索结果显示条数
sel = driver.find_element_by_xpath('//*[@id="nr"]')
Select(sel).select_by_value('50')  # 显示50条
# ……
sleep(2)
driver.quit()

```

Select类用于定位select标签。

select_by_value() 方法用于定位下接选项中的value值。

### 文件上传

对于通过input标签实现的上传功能，可以将其看作是一个输入框，即通过send_keys()指定本地文件路径的方式实现文件上传。

创建upfile.html文件，代码如下：
```
<html>
<head>
<meta http-equiv="content-type" content="text/html;charset=utf-8" />
<title>upload_file</title>
<link href="http://cdn.bootcss.com/bootstrap/3.3.0/css/bootstrap.min.css" rel="stylesheet" />
</head>
<body>
  <div class="row-fluid">
    <div class="span6 well">
    <h3>upload_file</h3>
      <input type="file" name="file" />
    </div>
  </div>
</body>
<script src="http://cdn.bootcss.com/bootstrap/3.3.0/css/bootstrap.min.js"></scrip>
</html>
```
通过浏览器打开upfile.html文件

接下来通过send_keys()方法来实现文件上传。
```
from selenium import webdriver
import os
import time

driver = webdriver.Chrome()
file_path = 'file:///' + os.path.abspath('upfile.html')
driver.get(file_path)

# 定位上传按钮，添加本地文件
driver.find_element_by_name("file").send_keys('C:\\1.json')
time.sleep(2)
driver.quit()
```
###  cookie操作

WebDriver提供了操作Cookie的相关方法，可以读取、添加和删除cookie信息。

WebDriver操作cookie的方法：

- get_cookies()： 获得所有cookie信息。

- get_cookie(name)： 返回字典的key为“name”的cookie信息。

- add_cookie(cookie_dict) ： 添加cookie。“cookie_dict”指字典对象，必须有name 和value 值。

- delete_cookie(name,optionsString)：删除cookie信息。“name”是要删除的cookie的名称，“optionsString”是该cookie的选项，目前支持的选项包括“路径”，“域”。

- delete_all_cookies()： 删除所有cookie信息。

下面通过get_cookies()来获取当前浏览器的cookie信息。
```
from selenium import webdriver

driver = webdriver.Chrome()
driver.get("http://www.youdao.com")

# 获得cookie信息
cookie= driver.get_cookies()
# 将获得cookie的信息打印
# print(cookie)
for co in cookie:
    print(co)

driver.quit()
```
运行结果如下：
```
{'domain': '.youdao.com', 'expiry': 2494590734.754279, 'httpOnly': False, 'name': 'OUTFOX_SEARCH_USER_ID', 'path': '/', 'secure': False, 'value': '1267639462@140.143.13.152'}
{'domain': '.youdao.com', 'httpOnly': False, 'name': 'DICT_UGC', 'path': '/', 'secure': False, 'value': 'be3af0da19b5c5e6aa4e17bd8d90b28a|'}
{'domain': '.youdao.com', 'httpOnly': False, 'name': 'JSESSIONID', 'path': '/', 'secure': False, 'value': 'abcbpPPBkALbF4DFQBkIw'}
{'domain': 'www.youdao.com', 'httpOnly': False, 'name': '___rl__test__cookies', 'path': '/', 'secure': False, 'value': '1548510735530'}
{'domain': '.youdao.com', 'expiry': 1611582735, 'httpOnly': False, 'name': 'OUTFOX_SEARCH_USER_ID_NCOO', 'path': '/', 'secure': False, 'value': '386788178.03478056'}

```
从执行结果可以看出，cookie数据是以字典的形式进行存放的。知道了cookie的存放形式，接下来我们就可以按照这种形式向浏览器中写入cookie信息。
```
from selenium import webdriver

driver = webdriver.Chrome()
driver.get("http://www.youdao.com")

# 向cookie的name 和value中添加会话信息
driver.add_cookie({'name': 'key-aaaaaaa', 'value': 'value-bbbbbb'})

# 遍历cookies中的name 和value信息并打印，当然还有上面添加的信息
for cookie in driver.get_cookies():
    print("%s -> %s" % (cookie['name'], cookie['value']))

driver.quit()
```
输出结果：
```
key-aaaaaaa -> value-bbbbbb
OUTFOX_SEARCH_USER_ID -> -782777771@140.143.13.152
DICT_UGC -> be3af0da19b5c5e6aa4e17bd8d90b28a|
JSESSIONID -> abc9gSVPkxH8neIBRCkIw
___rl__test__cookies -> 1548511001792
OUTFOX_SEARCH_USER_ID_NCOO -> 1032700391.184119
```

从执行结果可以看到，第一条cookie信息是在脚本执行过程中通过add_cookie()方法添加的。通过遍历得到所有的cookie信息，从而找到key为“name”和“value”的特定cookie的value。

###  调用JavaScript代码

虽然WebDriver提供了操作浏览器的前进和后退方法，但对于浏览器滚动条并没有提供相应的操作方法。在这种情况下，就可以借助JavaScript来控制浏览器的滚动条。WebDriver提供了execute_script()方法来执行JavaScript代码。

用于调整浏览器滚动条位置的JavaScript代码如下：

<!-- window.scrollTo(左边距,上边距); -->
window.scrollTo(0,450);
window.scrollTo()方法用于设置浏览器窗口滚动条的水平和垂直位置。方法的第一个参数表示水平的左间距，第二个参数表示垂直的上边距。其代码如下：
```
from selenium import webdriver
from time import sleep

# 访问百度
driver=webdriver.Chrome()
driver.get("http://www.baidu.com")

# 设置浏览器窗口大小
driver.set_window_size(700, 700)

# 搜索
driver.find_element_by_id("kw").send_keys("selenium")
driver.find_element_by_id("su").click()
sleep(2)

# 通过javascript设置浏览器窗口的滚动条位置
js="window.scrollTo(100,450);"
driver.execute_script(js)
sleep(3)

driver.quit()
```

通过浏览器打开百度进行搜索，并且提前通过set_window_size()方法将浏览器窗口设置为固定宽高显示，目的是让窗口出现水平和垂直滚动条。然后通过execute_script()方法执行JavaScripts代码来移动滚动条的位置。

###  窗口截图

自动化用例是由程序去执行的，因此有时候打印的错误信息并不十分明确。如果在脚本执行出错的时候能对当前窗口截图保存，那么通过图片就可以非常直观地看出出错的原因。WebDriver提供了截图函数get_screenshot_as_file()来截取当前窗口。
```
from selenium import webdriver
from time import sleep

driver = webdriver.Firefox()
driver.get('http://www.baidu.com')

driver.find_element_by_id('kw').send_keys('selenium')
driver.find_element_by_id('su').click()
sleep(2)

# 截取当前窗口，并指定截图图片的保存位置
driver.get_screenshot_as_file("D:\\baidu_img.jpg")

driver.quit()
```
脚本运行完成后打开C盘，就可以找到baidu_img.jpg图片文件了。
