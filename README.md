
### Selenium中有三种弹框，本文介绍了处理三种弹框的方法


#### 一、Selenium三种弹框


alert：用来提示，显示一个带有指定消息和确认按钮的警告框
confirm：用于确认，显示一个带有指定消息和确定及取消按钮的对话框
prompt：用于用户输入内容，显示可进行输入的对话框
这三种弹框不是html的页面元素，而是javascript的控件，所以不能用传统的方法去操作，需要用另外的方法操作，下面介绍处理三种弹框的方法已经弹框的属性：
（1）accept()：用于确认，适用三种弹框
（2）dismiss()：用于取消，适用三种弹框
（3）send\_keys()：仅适用于prompt方法，用于输入文本
（4）text：用于获取提示的文本值


#### 二、定义form表单


定义三个超链接，点击分别弹出不同弹框



```


|  | <a href="javascript:alert('提示框')" id="alert">Alerta><br> |
| --- | --- |
|  | <a href="javascript:confirm('确认删除吗')" id="confirm">Confirma><br> |
|  |  |
|  | prompt返回内容存在变量age中，取到返回值后将变量写入页面 |
|  | <a href="javascript:var age=prompt('请输入年龄');document.write(age)" id="prompt">Prompta><br> |


```

界面如下


#### 三、表单测试


##### 1、alert测试


（1）用例1：点击超链接alert
结果1：弹出alert类型弹窗
![image](https://img2024.cnblogs.com/blog/3493633/202410/3493633-20241028232344110-591975330.png)
自动化代码:直接调用WebElement类中的click()方法



```


|  | self.driver.find_element(By.ID,"alert").click()#点击超链接alert |
| --- | --- |


```

（2）用例2：弹框中点击确定
结果2：弹框消失
![image](https://img2024.cnblogs.com/blog/3493633/202410/3493633-20241028232745659-45554083.png)


自动化代码：
**首先需要将Webdriver的作用域从主窗口切到弹框上，操作三种弹框：alert、confirm、prompt前，都需要执行该操作，用到的方法是self.driver.switch\_to.alert**，第二步就可以在弹框中操作，点击确定，用到的方法是accept()



```


|  | alert=self.driver.switch_to.alert#由主窗口切换到alert,返回一个alert对象 |
| --- | --- |
|  | sleep(2) |
|  | #print(alert.text) |
|  | alert.accept()#点击确定 |
|  | sleep(2) |


```

##### 2、confirm测试


（1）用例1:点击超链接confirm
结果2：弹出confirm类型的弹框
![image](https://img2024.cnblogs.com/blog/3493633/202410/3493633-20241028232943464-2047030131.png)


自动化代码：同样直接调用WebElement类中的click()方法



```


|  | self.driver.find_element(By.ID,"confirm").click()#点击confirm超链接 |
| --- | --- |


```

（2）用例2：点击确定
结果2：弹框消失
![image](https://img2024.cnblogs.com/blog/3493633/202410/3493633-20241028232745659-45554083.png)
自动化代码：
用到的方法是accept()



```


|  | confirm=self.driver.switch_to.alert#切换至confirm弹框 |
| --- | --- |
|  | sleep(2) |
|  | print(confirm.text)#打印弹窗中文本 |
|  | confirm.accept()#点击确定 |
|  | sleep(2) |


```

（3）用例3：点击取消
结果3：弹框消失
![image](https://img2024.cnblogs.com/blog/3493633/202410/3493633-20241028232745659-45554083.png)
自动化代码：用到的方法是dismiss()



```


|  | confirm.dismiss()#点击取消 |
| --- | --- |
|  | sleep(2) |


```

##### 3、prompt测试


（1）用例1：点击超链接prompt
结果1：弹出prompt类型弹框
![image](https://img2024.cnblogs.com/blog/3493633/202410/3493633-20241028234334451-318912144.png)


自动化代码：直接调用WebElement类中的click()方法



```


|  | self.driver.find_element(By.ID,"prompt").click() |
| --- | --- |


```

（2）用例2：输入内容并确认
用例2：文本成功输入并写入页面
![image](https://img2024.cnblogs.com/blog/3493633/202410/3493633-20241029214248533-1700771415.png)


![image](https://img2024.cnblogs.com/blog/3493633/202410/3493633-20241029214134262-1699549690.png)


自动化代码：
输入文本用到的是send\_keys()方法，点击确定用到的是accept()



```


|  | prompt=self.driver.switch_to.alert#切换到prompt弹框 |
| --- | --- |
|  | sleep(2) |
|  | prompt.send_keys('12') |
|  | sleep(2) |
|  | prompt.accept() |
|  | sleep(2) |


```

#### 三、总代码



点击查看代码

```


|  | from selenium import webdriver |
| --- | --- |
|  | from time import sleep |
|  | import os |
|  | from selenium.webdriver.common.by import By |
|  |  |
|  | class TestCase(object): |
|  | def __init__(self): |
|  | self.driver=webdriver.Edge() |
|  | path=os.path.dirname(os.path.abspath(__file__)) |
|  | file_path="file:///"+path+"/form3.html" |
|  | self.driver.get(file_path) |
|  |  |
|  | def test_alert(self): |
|  | self.driver.find_element(By.ID,"alert").click()#点击alert超链接 |
|  | alert=self.driver.switch_to.alert#切换到弹窗,返回一个对象 |
|  | sleep(2) |
|  | print(alert.text)#可以打印text内容确定 |
|  | alert.dismiss()#点击确定 |
|  | sleep(2) |
|  |  |
|  | def test_confirm(self): |
|  | self.driver.find_element(By.ID,"confirm").click()#点击confirm超链接 |
|  | confirm=self.driver.switch_to.alert#切换至confirm弹框 |
|  | sleep(2) |
|  | print(confirm.text)#打印弹窗中文本 |
|  | confirm.accept()#点击确定 |
|  | sleep(2) |
|  | confirm.dismiss()#点击取消 |
|  | sleep(2) |
|  |  |
|  | def test_prompt(self): |
|  | self.driver.find_element(By.ID,"prompt").click() |
|  | prompt=self.driver.switch_to.alert#切换到prompt弹框 |
|  | sleep(2) |
|  | prompt.send_keys('12') |
|  | sleep(2) |
|  | prompt.dismiss() |
|  | sleep(2) |
|  |  |
|  | if __name__=="__main__": |
|  | case=TestCase() |
|  | #case.test_alert() |
|  | #case.test_confirm() |
|  | case.test_prompt() |


```


 本博客参考[veee加速器](https://youhaochi.com)。转载请注明出处！
