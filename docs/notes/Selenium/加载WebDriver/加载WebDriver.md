# 1. 加载WebDriver
## 1.1. Chrome

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
import time

# chrome-test路径
chrome_testing_path = r"D:\chrome-for-test\chrome-win64\chrome.exe"

# chromedriver/
chromedriver_path = r"D:\chrome-for-test\chromedriver-win64\chromedriver.exe"

# 设置chrome选项
options = webdriver.ChromeOptions()
options.binary_location = chrome_testing_path
options.add_experimental_option('detach', True)

# 设置webdriver服务
service = Service(chromedriver_path)


def main():
    driver = webdriver.Chrome(service=service, options=options)
    driver.get('https://www.baidu.com')
    time.sleep(5)
    driver.quit()


if __name__ == '__main__':
    main()

```