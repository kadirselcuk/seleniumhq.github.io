---
title: "Cliente do WebDriver remoto"
weight: 2
---

Para executar um cliente WebDriver remoto, primeiro precisamos nos conectar ao RemoteWebDriver.
Fazemos isso apontando a URL para o endereço do servidor que está executando nossos testes.
Para personalizar nossa configuração, definimos os recursos desejados.
Abaixo está um exemplo de como instanciar um objeto WebDriver remoto
apontando para nosso servidor remoto da web, _www.example.com_,
executando nossos testes no Firefox.

{{< code-tab >}}
  {{< code-panel language="java" >}}
FirefoxOptions firefoxOptions = new FirefoxOptions();
WebDriver driver = new RemoteWebDriver(new URL("http://www.example.com"), firefoxOptions);
driver.get("http://www.google.com");
driver.quit();
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
from selenium import webdriver

firefox_options = webdriver.FirefoxOptions()
driver = webdriver.Remote(
    command_executor='http://www.example.com',
    options=firefox_options
)
driver.get("http://www.google.com")
driver.quit() 
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
 FirefoxOptions firefoxOptions = new FirefoxOptions();
 IWebDriver driver = new RemoteWebDriver(new Uri("http://www.example.com"), firefoxOptions);
 driver.Navigate().GoToUrl("http://www.google.com");
 driver.Quit();
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
require 'selenium-webdriver'

driver = Selenium::WebDriver.for :remote, url: "http://www.example.com", desired_capabilities: :firefox
driver.get "http://www.google.com"
driver.close
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
const { Builder, Capabilities } = require("selenium-webdriver");
var capabilities = Capabilities.firefox();
(async function helloSelenium() {
    let driver = new Builder()
        .usingServer("http://example.com")   
        .withCapabilities(capabilities)
        .build();
    try {
        await driver.get('http://www.google.com');
    } finally {
        await driver.quit();
    }
})();  
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
firefoxOptions = FirefoxOptions()
driver: WebDriver = new RemoteWebDriver(new URL("http://www.example.com"), firefoxOptions)
driver.get("http://www.google.com")
driver.quit()
  {{< / code-panel >}}
{{< / code-tab >}}


Para personalizar ainda mais nossa configuração de teste, podemos adicionar outros recursos desejados.


## Opções do browser

Por exemplo, suponha que você queira executar o Chrome no Windows XP,
usando o Chrome versão 67:

{{< code-tab >}}
  {{< code-panel language="java" >}}
ChromeOptions chromeOptions = new ChromeOptions();
chromeOptions.setCapability("browserVersion", "67");
chromeOptions.setCapability("platformName", "Windows XP");
WebDriver driver = new RemoteWebDriver(new URL("http://www.example.com"), chromeOptions);
driver.get("http://www.google.com");
driver.quit();
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
from selenium import webdriver

chrome_options = webdriver.ChromeOptions()
chrome_options.set_capability("browserVersion", "67")
chrome_options.set_capability("platformName", "Windows XP")
driver = webdriver.Remote(
    command_executor='http://www.example.com',
    options=chrome_options
)
driver.get("http://www.google.com")
driver.quit()  
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
var chromeOptions = new ChromeOptions();
chromeOptions.BrowserVersion = "67";
chromeOptions.PlatformName = "Windows XP";
IWebDriver driver = new RemoteWebDriver(new Uri("http://www.example.com"), chromeOptions);
driver.Navigate().GoToUrl("http://www.google.com");
driver.Quit();
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
caps = Selenium::WebDriver::Remote::Capabilities.chrome
caps.platform = Windows XP
caps.version = 67

driver = Selenium::WebDriver.for :remote, :url => "http://www.example.com", :desired_capabilities => caps
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
const { Builder } = require("selenium-webdriver");
const chrome = require("selenium-webdriver/chrome");
let opts = new chrome.Options();
opts.setAcceptInsecureCerts(true);
opts.setBrowserVersion('67');
opts.setPlatform('Windows XP');
(async function helloSelenium() {
    let driver = new Builder()
        .usingServer("http://example.com")
        .forBrowser('chrome')
        .setChromeOptions(opts)
        .build();
    try {
        await driver.get('http://www.google.com');
    }
    finally {
        await driver.quit();
    }
})();
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
val chromeOptions = ChromeOptions()
chromeOptions.setCapability("browserVersion", "67")
chromeOptions.setCapability("platformName", "Windows XP")
val driver: WebDriver = new RemoteWebDriver(new URL("http://www.example.com"), chromeOptions)
driver.get("http://www.google.com")
driver.quit()
  {{< / code-panel >}}
{{< / code-tab >}}


## Detector de arquivo local

O Detector de Arquivo Local permite a transferência de arquivos da máquina do cliente para o servidor remoto. Por exemplo, se um teste precisa carregar um
arquivo para um aplicativo da web, um WebDriver remoto pode transferir automaticamente
o arquivo da máquina local para o servidor web remoto durante
o tempo de execução. Isso permite que o arquivo seja carregado da máquina remota
executando o teste. Não é habilitado por padrão e pode ser habilitado em
da seguinte forma:

{{< code-tab >}}
  {{< code-panel language="java" >}}
driver.setFileDetector(new LocalFileDetector());
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
from selenium.webdriver.remote.file_detector import LocalFileDetector

driver.file_detector = LocalFileDetector()
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
var allowsDetection = this.driver as IAllowsFileDetection;
if (allowsDetection != null)
{
   allowsDetection.FileDetector = new LocalFileDetector();
}
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
@driver.file_detector = lambda do |args|
  # args => ["/path/to/file"]
  str = args.first.to_s
  str if File.exist?(str)
end
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
var remote = require('selenium-webdriver/remote');
driver.setFileDetector(new remote.FileDetector);  
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
driver.fileDetector = LocalFileDetector()
  {{< / code-panel >}}
{{< / code-tab >}}

Assim que o código acima for definido, você pode fazer upload de um arquivo em seu teste da seguinte maneira:

{{< code-tab >}}
  {{< code-panel language="java" >}}
driver.get("http://sso.dev.saucelabs.com/test/guinea-file-upload");
WebElement upload = driver.findElement(By.id("myfile"));
upload.sendKeys("/Users/sso/the/local/path/to/darkbulb.jpg");
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
driver.get("http://sso.dev.saucelabs.com/test/guinea-file-upload")

driver.find_element(By.ID, "myfile").send_keys("/Users/sso/the/local/path/to/darkbulb.jpg")
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
driver.Navigate().GoToUrl("http://sso.dev.saucelabs.com/test/guinea-file-upload");
IWebElement upload = driver.FindElement(By.Id("myfile"));
upload.SendKeys(@"/Users/sso/the/local/path/to/darkbulb.jpg");
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
@driver.navigate.to "http://sso.dev.saucelabs.com/test/guinea-file-upload"
    element = @driver.find_element(:id, 'myfile')
    element.send_keys "/Users/sso/SauceLabs/sauce/hostess/maitred/maitred/public/images/darkbulb.jpg"
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
driver.get("http://sso.dev.saucelabs.com/test/guinea-file-upload");
var upload = driver.findElement(By.id("myfile"));
upload.sendKeys("/Users/sso/the/local/path/to/darkbulb.jpg");  
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
driver.get("http://sso.dev.saucelabs.com/test/guinea-file-upload")
val upload: WebElement = driver.findElement(By.id("myfile"))
upload.sendKeys("/Users/sso/the/local/path/to/darkbulb.jpg")
  {{< / code-panel >}}
{{< / code-tab >}}
