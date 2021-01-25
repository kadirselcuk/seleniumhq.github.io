---
title: "定位元素"
weight: 3
---

### 定位元素

使用 WebDriver 时要学习的最基本的技术之一是如何查找页面上的元素。
WebDriver 提供了许多内置的选择器类型，其中包括根据 id 属性查找元素:

{{< code-tab >}}
  {{< code-panel language="java" >}}
WebElement cheese = driver.findElement(By.id("cheese"));
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
driver.find_element(By.ID, "cheese")
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
IWebElement element = driver.FindElement(By.Id("cheese"));
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
cheese = driver.find_element(id: 'cheese')
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
const cheese = driver.findElement(By.id('cheese'));
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
val cheese: WebElement = driver.findElement(By.id("cheese"))
  {{< / code-panel >}}
{{< / code-tab >}}

如示例所示，在 WebDriver 中定位元素是在 `WebDriver` 实例对象上完成的。
 `findElement(By)` 方法返回另一个基本对象类型 `WebElement`。

* `WebDriver` 代表浏览器
* `WebElement` 表示特定的 DOM 节点（控件，例如链接或输入栏等）

一旦你已经找到一个元素的引用，你可以通过对该对象实例使用相同的调用来缩小搜索范围：

{{< code-tab >}}
  {{< code-panel language="java" >}}
WebElement cheese = driver.findElement(By.id("cheese"));
WebElement cheddar = cheese.findElement(By.id("cheddar"));
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
cheese = driver.find_element(By.ID, "cheese")
cheddar = cheese.find_elements_by_id("cheddar")
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
IWebElement cheese = driver.FindElement(By.Id("cheese"));
IWebElement cheddar = cheese.FindElement(By.Id("cheddar"));
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
cheese = driver.find_element(id: 'cheese')
cheddar = cheese.find_element(id: 'cheddar')
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
const cheese = driver.findElement(By.id('cheese'));
const cheddar = cheese.findElement(By.id('cheddar'));
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
val cheese = driver.findElement(By.id("cheese"))
val cheddar = cheese.findElement(By.id("cheddar"))
  {{< / code-panel >}}
{{< / code-tab >}}

你可以这样做是因为， _WebDriver_ 和 _WebElement_ 类型都实现了 [_搜索上下文_](//seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/SearchContext.html) 接口。在 WebDriver 中，这称为 _基于角色的接口_。基于角色的接口允许你确定特定的驱动程序实现是否支持给定的功能。这些接口定义得很清楚，并且尽量只承担单一的功能。你可以阅读更多关于 WebDriver 的设计，以及在 WebDriver 中有哪些角色被支持，在[其他被命名的部分](#)。
<!-- TODO: A new section needs to be created for the above.-->

因此，上面使用的 _By_ 接口也支持许多附加的定位器策略。嵌套查找可能不是最有效的定位 cheese 的策略，因为它需要向浏览器发出两个单独的命令：首先在 DOM 中搜索 id 为“cheese”的元素，然后在较小范围的上下文中搜索“cheddar”。

为了稍微提高性能，我们应该尝试使用一个更具体的定位器：WebDriver 支持通过 CSS 定位器查找元素，我们可以将之前的两个定位器合并到一个搜索里面:

{{< code-tab >}}
  {{< code-panel language="java" >}}
driver.findElement(By.cssSelector("#cheese #cheddar"));
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
cheddar = driver.find_element_by_css_selector("#cheese #cheddar")
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
driver.FindElement(By.CssSelector("#cheese #cheddar"));
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
driver.find_element(css: '#cheese #cheddar')
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
const cheddar = driver.findElement(By.css('#cheese #cheddar'));
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
driver.findElement(By.cssSelector("#cheese #cheddar"))
  {{< / code-panel >}}
{{< / code-tab >}}

### 定位多个元素

我们正在处理的文本中可能会有一个我们最喜欢的奶酪的订单列表：

```html
<ol id=cheese>
 <li id=cheddar>…
 <li id=brie>…
 <li id=rochefort>…
 <li id=camembert>…
</ol>
```

因为有更多的奶酪无疑是更好的，但是单独检索每一个项目是很麻烦的，检索奶酪的一个更好的方式是使用复数版本 `findElements(By)` 。此方法返回 web 元素的集合。如果只找到一个元素，它仍然返回(一个元素的)集合。如果没有元素被定位器匹配到，它将返回一个空列表。

{{< code-tab >}}
  {{< code-panel language="java" >}}
List<WebElement> muchoCheese = driver.findElements(By.cssSelector("#cheese li"));
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
mucho_cheese = driver.find_elements_by_css_selector("#cheese li")
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
IReadOnlyList<IWebElement> muchoCheese = driver.FindElements(By.CssSelector("#cheese li"));
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
mucho_cheese = driver.find_elements(css: '#cheese li')
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
const muchoCheese = driver.findElements(By.css('#cheese li'));
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
val muchoCheese: List<WebElement>  = driver.findElements(By.cssSelector("#cheese li"))
  {{< / code-panel >}}
{{< / code-tab >}}

### 元素选择策略

在 WebDriver 中有 8 种不同的内置元素定位策略：

| 定位器 Locator | 描述 |
| -------- | ---------- |
| class name | 定位class属性与搜索值匹配的元素（不允许使用复合类名） |
| css selector | 定位 CSS 选择器匹配的元素 |
| id | 定位 id 属性与搜索值匹配的元素 |
| name | 定位 name 属性与搜索值匹配的元素 |
| link text | 定位link text可视文本与搜索值完全匹配的锚元素 |
| partial link text | 定位link text可视文本部分与搜索值部分匹配的锚点元素。如果匹配多个元素，则只选择第一个元素。 |
| tag name | 定位标签名称与搜索值匹配的元素 |
| xpath | 定位与 XPath 表达式匹配的元素 |

### 使用选择器的提示

一般来说，如果 HTML 的 id 是可用的、唯一的且是可预测的，那么它就是在页面上定位元素的首选方法。它们的工作速度非常快，可以避免复杂的 DOM 遍历带来的大量处理。

如果没有唯一的 id，那么最好使用写得好的 CSS 选择器来查找元素。XPath 和 CSS 选择器一样好用，但是它语法很复杂，并且经常很难调试。尽管 XPath 选择器非常灵活，但是他们通常未经过浏览器厂商的性能测试，并且运行速度很慢。

基于链接文本和部分链接文本的选择策略有其缺点，即只能对链接元素起作用。此外，它们在 WebDriver 内部调用 XPath 选择器。

标签名可能是一种危险的定位元素的方法。页面上经常出现同一标签的多个元素。这在调用 _findElements(By)_ 方法返回元素集合的时候非常有用。

建议您尽可能保持定位器的紧凑性和可读性。使用 WebDriver 遍历 DOM 结构是一项性能花销很大的操作，搜索范围越小越好。

## 相对定位

在**Selenium 4**中带来了相对定位这个新功能，在以前的版本中被称之为"好友定位 (Friendly Locators)"。
它可以帮助你通过某些元素作为参考来定位其附近的元素。
现在可用的相对定位有：

* *above* 元素上
* *below* 元素下
* *toLeftOf* 元素左
* *toRightOf* 元素右
* *near* 附近

_findElement_ 方法现在支持`witTagName()`新方法其可返回RelativeLocator相对定位对象。

### 如何工作

Selenium是通过使用JavaScript函数
[getBoundingClientRect()](https://developer.mozilla.org/en-US/docs/Web/API/Element/getBoundingClientRect)
来查找相对元素的。这个函数能够返回对应元素的各种属性例如：右，左，下，上。

通过下面的例子我们来理解一下关于相对定位的使用

![Relative Locators](/images/relative_locators.png?width=400px)

### above() 元素上

返回当前指定元素位置上方的WebElement对象

{{< code-tab >}}
  {{< code-panel language="java" >}}
//import static org.openqa.selenium.support.locators.RelativeLocator.withTagName;
WebElement passwordField= driver.findElement(By.id("password"));
WebElement emailAddressField = driver.findElement(withTagName("input")
                                                  .above(passwordField));
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
#from selenium.webdriver.support.relative_locator import with_tag_name
passwordField = driver.find_element(By.ID, "password")
emailAddressField = driver.find_element(with_tag_name("input").above(passwordField))
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
//using static OpenQA.Selenium.RelativeBy;
IWebElement passwordField = driver.FindElement(By.Id("password"));
IWebElement emailAddressField = driver.FindElement(WithTagName("input")
                                                   .Above(passwordField));
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
password_field= driver.find_element(:id, "password")
email_address_field = driver.find_element(relative: {tag_name: 'input', above:password_field})
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
let passwordField = driver.findElement(By.id('password'));
let emailAddressField = await driver.findElements(withTagName('input').above(passwordField));
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
val passwordField = driver.findElement(By.id("password"))
val emailAddressField = driver.findElement(withTagName("input").above(passwordField))
  {{< / code-panel >}}
{{< / code-tab >}}


### below() 元素下

返回当前指定元素位置下方的WebElement对象


{{< code-tab >}}
  {{< code-panel language="java" >}}
//import static org.openqa.selenium.support.locators.RelativeLocator.withTagName;
WebElement emailAddressField= driver.findElement(By.id("email"));
WebElement passwordField = driver.findElement(withTagName("input")
	                                          .below(emailAddressField));
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
#from selenium.webdriver.support.relative_locator import with_tag_name
emailAddressField = driver.find_element(By.ID, "email")
passwordField = driver.find_element(with_tag_name("input").below(emailAddressField))
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
//using static OpenQA.Selenium.RelativeBy;  
IWebElement emailAddressField = driver.FindElement(By.Id("email"));
IWebElement passwordField = driver.FindElement(WithTagName("input")
                                               .Below(emailAddressField));
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
email_address_field= driver.find_element(:id, "email")
password_field = driver.find_element(relative: {tag_name: 'input', below: email_address_field})
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
let emailAddressField = driver.findElement(By.id('email'));
let passwordField = await driver.findElements(withTagName('input').below(emailAddressField));
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
val emailAddressField = driver.findElement(By.id("email"))
val passwordField = driver.findElement(withTagName("input").below(emailAddressField))
  {{< / code-panel >}}
{{< / code-tab >}}


### toLeftOf() 元素左

返回当前指定元素位置左方的WebElement对象

{{< code-tab >}}
  {{< code-panel language="java" >}}
//import static org.openqa.selenium.support.locators.RelativeLocator.withTagName;
WebElement submitButton= driver.findElement(By.id("submit"));
WebElement cancelButton= driver.findElement(withTagName("button")
                                            .toLeftOf(submitButton));   
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
#from selenium.webdriver.support.relative_locator import with_tag_name
submitButton = driver.find_element(By.ID, "submit")
cancelButton = driver.find_element(with_tag_name("button").
                                   to_left_of(submitButton))
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
//using static OpenQA.Selenium.RelativeBy;
IWebElement submitButton = driver.FindElement(By.Id("submit"));
IWebElement cancelButton = driver.FindElement(WithTagName("button")
                                              .LeftOf(submitButton));
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
submit_button= driver.find_element(:id, "submit")
cancel_button = driver.find_element(relative: {tag_name: 'button', left:submit_button})
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
let submitButton = driver.findElement(By.id("submit"));
let cancelButton = await driver.findElements(withTagName("button").toLeftOf(submitButton));
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
val submitButton= driver.findElement(By.id("submit"))
val cancelButton= driver.findElement(withTagName("button").toLeftOf(submitButton))
  {{< / code-panel >}}
{{< / code-tab >}}


### toRightOf() 元素右

返回当前指定元素位置右方的WebElement对象

{{< code-tab >}}
  {{< code-panel language="java" >}}
//import static org.openqa.selenium.support.locators.RelativeLocator.withTagName;
WebElement cancelButton= driver.findElement(By.id("cancel"));
WebElement submitButton= driver.findElement(withTagName("button")
                                            .toRightOf(cancelButton));
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
#from selenium.webdriver.support.relative_locator import with_tag_name
cancelButton = driver.find_element(By.ID, "cancel")
submitButton = driver.find_element(with_tag_name("button").
                                   to_right_of(cancelButton))
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
//using static OpenQA.Selenium.RelativeBy;
IWebElement cancelButton = driver.FindElement(By.Id("cancel"));
IWebElement submitButton = driver.FindElement(WithTagName("button")
                                              .RightOf(cancelButton));
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
cancel_button = driver.find_element(:id, "cancel")
submit_button = driver.find_element(relative: {tag_name: 'button', right:cancel_button})
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
let cancelButton = driver.findElement(By.id('cancel'));
let submitButton = await driver.findElements(withTagName('button').toRightOf(cancelButton));
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
val cancelButton= driver.findElement(By.id("cancel"))
val submitButton= driver.findElement(withTagName("button").toRightOf(cancelButton))
  {{< / code-panel >}}
{{< / code-tab >}}

### near() 附近

返回当前指定元素位置附近大约`px50`50像素的WebElement对象

{{< code-tab >}}
  {{< code-panel language="java" >}}
//import static org.openqa.selenium.support.locators.RelativeLocator.withTagName;
WebElement emailAddressLabel= driver.findElement(By.id("lbl-email"));
WebElement emailAddressField = driver.findElement(withTagName("input")
                                                  .near(emailAddressLabel));
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
#from selenium.webdriver.support.relative_locator import with_tag_name
emailAddressLabel = driver.find_element(By.ID, "lbl-email")
emailAddressField = driver.find_element(with_tag_name("input").
                                       near(emailAddressLabel))
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
//using static OpenQA.Selenium.RelativeBy;
IWebElement emailAddressLabel = driver.FindElement(By.Id("lbl-email"));
IWebElement emailAddressField = driver.FindElement(WithTagName("input")
                                                   .Near(emailAddressLabel));
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
email_address_label = driver.find_element(:id, "lbl-email")
email_address_field = driver.find_element(relative: {tag_name: 'input', near: email_address_label})
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
let emailAddressLabel = driver.findElement(By.id("lbl-email"));
let emailAddressField = await driver.findElements(withTagName("input").near(emailAddressLabel));
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
val emailAddressLabel = driver.findElement(By.id("lbl-email"))
val emailAddressField = driver.findElement(withTagName("input").near(emailAddressLabel))
  {{< / code-panel >}}
{{< / code-tab >}}
