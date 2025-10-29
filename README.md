### 1.简介

前边几篇文章是宏哥自己在本地弄了一个单选和多选的demo，给小伙伴或童鞋们演示了一下如何使用playwright来处理单选按钮和多选按钮进行自动化测试，想必大家都已经掌握的八九不离十了吧。这一篇其实也很简单，主要是分两部分内容来讲解和分享的。一部分，宏哥是利用JQueryUI网站里的单选和多选按钮进行实战，主要是循环遍历，对前边内容进行梳理和回顾。另一部分就是宏哥在网上找了一个问卷调查例子，运用前边所学的知识趁热打铁地给小伙伴或童鞋们来演示一下。前边的文章中的一些单选和多选的基本概念都介绍了，这里就不做赘述了。直接上项目进行实战。

### 2.JQueryUI网站

[https://www.jq22.com/](https://github.com)  这个是宏哥又找到的一个网站，不错的，有源码。进入后可以搜索你要演示的demo。

#### 2.1被测网址

1.被测网址的地址：

为了方便演示，宏哥直接将其iframe中的url拿出来了，否则你的定位到iframe，然后才能定位里边的元素。这个坑宏哥之前遇到过一次。这里再次提醒一下。

[https://www.jq22.com/demo/inputStyle201703310052](https://github.com):[橘子云加速器](https://91biji.org)

2.网页如下图：

![](https://img2023.cnblogs.com/blog/1232840/202311/1232840-20231102085818458-1083323957.png)

#### 2.2代码设计

宏哥这里只演示单选的遍历，复选的有兴趣的童鞋可以自己试一下。根据demo中的遍历思路进行代码设计。如下图所示：

![](https://img2024.cnblogs.com/blog/1232840/202501/1232840-20250121165549843-1408884004.png)

#### 2.3参考代码

```
package com.bjhg.playwright;

import java.util.List;

import com.microsoft.playwright.Browser;
import com.microsoft.playwright.BrowserContext;
import com.microsoft.playwright.BrowserType;
import com.microsoft.playwright.Locator;
import com.microsoft.playwright.Page;
import com.microsoft.playwright.Playwright;

/**
 * @author 北京-宏哥
 * 
 * @公众号:北京宏哥（微信搜索，关注宏哥，提前解锁更多测试干货）
 * 
 * 《刚刚问世》系列初窥篇-Java+Playwright自动化测试-28- 操作单选和多选按钮 - 中篇（详细教程）
 *
 * 2025年01月26日
 */
public class Test_Radio {
    
    public static void main(String[] args) {
        try (Playwright playwright = Playwright.create()) {
              //1.使用chromium浏览器，# 浏览器配置，设置以GUI模式启动Chrome浏览器（要查看浏览器UI，在启动浏览器时传递 headless=false 标志。您还可以使用 slowMo 来减慢执行速度。
              Browser browser = playwright.chromium().launch(new BrowserType.LaunchOptions().setHeadless(false).setSlowMo(3000));
              //2.创建context
              BrowserContext context = browser.newContext();
              //创建page
              Page page = context.newPage();
              //3.浏览器访问demo
              page.navigate("https://www.jq22.com/demo/inputStyle201703310052");
              //4.定位所有单选按钮
              List radios = page.locator("[name='hot']").all();
              //遍历单选按钮
              for(Locator radio:radios){
                 //判断单选按钮是否选中，不选中才点击
                 if(!radio.isChecked()){
                     //点击单选按钮
                     radio.click(); 
                 }
              }
              System.out.println("Test Pass");
              //5.关闭page
              page.close();
              //6.关闭browser
              browser.close();
        }
    }
}
```

#### 2.4运行代码

1.运行代码，右键Run As->Java Application，就可以看到控制台输出，如下图所示：

![](https://img2024.cnblogs.com/blog/1232840/202501/1232840-20250121165653842-1111896393.png)

2.运行代码后电脑端的浏览器的动作（可以看到单选按钮挨个都被点到了）。如下图所示：

![](https://img2024.cnblogs.com/blog/1232840/202501/1232840-20250121165904419-1395048651.gif)

### 3.项目实战

#### 3.1问卷调查

1.测试问卷调查的地址：

[https://www.sojump.com/m/2792226.aspx/](https://github.com)

2.问卷页面，如下图所示：

![](https://img2020.cnblogs.com/blog/1232840/202110/1232840-20211018132830371-775475613.png)

#### 3.2答题思路

自动化测试答题思路，其实和前边单选多选的遍历差不多，具体思路如下：

1.首先找到所有单选和多选按钮的共同点。

2.使用共同点来定位单选和多选按钮，将其放在容器中。

3.利用for循环将其（单选和多选按钮）从容器中一一遍历出来，并进行逐个click。

#### 3.3代码设计

根据答题中的遍历思路进行代码设计如下图所示：

![](https://img2024.cnblogs.com/blog/1232840/202501/1232840-20250122153308636-53567262.png)

#### 3.4参考代码

```
package com.bjhg.playwright;

import java.util.List;

import com.microsoft.playwright.Browser;
import com.microsoft.playwright.BrowserContext;
import com.microsoft.playwright.BrowserType;
import com.microsoft.playwright.Locator;
import com.microsoft.playwright.Page;
import com.microsoft.playwright.Playwright;

/**
 * @author 北京-宏哥
 * 
 * @公众号:北京宏哥（微信搜索，关注宏哥，提前解锁更多测试干货）
 * 
 * 《刚刚问世》系列初窥篇-Java+Playwright自动化测试-28- 操作单选和多选按钮 - 中篇（详细教程）
 *
 * 2025年01月29日
 */
public class Test_Radio {
    
    public static void main(String[] args) {
        try (Playwright playwright = Playwright.create()) {
              //1.使用chromium浏览器，# 浏览器配置，设置以GUI模式启动Chrome浏览器（要查看浏览器UI，在启动浏览器时传递 headless=false 标志。您还可以使用 slowMo 来减慢执行速度。
              Browser browser = playwright.chromium().launch(new BrowserType.LaunchOptions().setHeadless(false).setSlowMo(3000));
              //2.创建context
              BrowserContext context = browser.newContext();
              //创建page
              Page page = context.newPage();
              //3.浏览器访问demo
              page.navigate("https://www.sojump.com/m/2792226.aspx");
              Thread.sleep(1000);
              //4.定位所有单选按钮
              List radios = page.locator("//*/div[@id='divQuestion']/fieldset/div/div/div/span/input/../a").all();
              //遍历单选按钮
              for(Locator radio:radios){
                //点击单选按钮
                  radio.click();
              }
              System.out.println("Test Pass");
              //5.关闭page
              page.close();
              //6.关闭browser
              browser.close();
        } catch (InterruptedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

#### 3.5运行代码

1.运行代码，右键Run As->Java Application，就可以看到控制台输出，如下图所示：

![](https://img2024.cnblogs.com/blog/1232840/202501/1232840-20250122153800511-1806416598.png)

2.运行代码后电脑端的浏览器的动作（可以看到所有题目的单选和多选全部点击一遍）。如下图所示：

![](https://img2024.cnblogs.com/blog/1232840/202501/1232840-20250122153841894-165397100.gif)

### 4.小结

#### 4.1画蛇添足

实践过程中，宏哥遇到一个报错，原就是画蛇添足，多写一段代码：判断按钮是否选中，结果导致报错：

Exception in thread "main" com.microsoft.playwright.PlaywrightException: Error { message='Error: Not a checkbox or radio button

意思是：不是单选或者多选按钮，宏哥F12查看果然不是，是一个input标签，如下图所示：

![](https://img2024.cnblogs.com/blog/1232840/202501/1232840-20250122154432355-89520288.png)

 因此需要将那个判断是否选中的代码取消之后，代码成功运行。

今天其实就是对前边单选和多选循环的一次总结和实践。其他的也没有新的东西。好了，今天时间也不早了，宏哥就讲解和分享到这里，感谢您耐心的阅读，希望对您有所帮助。
