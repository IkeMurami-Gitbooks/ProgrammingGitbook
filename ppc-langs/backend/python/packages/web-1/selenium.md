# Selenium

## Start Web Driver

```python
# selenium
from selenium import webdriver
from selenium.webdriver.firefox.options import Options

chrome_options = Options()
chrome_options.add_argument('--headless')
chrome_options.add_argument("--no-sandbox")
chrome_options.add_argument("--disable-dev-shm-usage")

self.webdriver = webdriver.Firefox(options=chrome_options)
```

## Скачивание chromedriver для версии нашего браузера&#x20;

```python
webdriver.Chrome(
    ChromeDriverManager().install(), options=chrome_options
)
```

## Переход на страницу

```python
import time

TIME_SLEEP = 1

# open target
self.webdriver.get(target.url)

# pause for rendering page
time.sleep(TIME_SLEEP)
```

## Работа со страницей

```python
parent_page_url = self.webdriver.current_url

for script in self.webdriver.find_elements_by_tag_name('script'):
    src = script.get_attribute('src')
    content_script = script.get_attribute('innerHTML')
```

## Открытие локального HTML-файла

```python
cj_target = os.path.join(CLICKJACKING_DIR, 'cj-target.html')
local_url = f'file://{cj_target}'

self.webdriver.get(local_url)
...
```

## PhantomJS: Запуск своего скрипта

```python
from selenium import webdriver

myscript = open('jsTestForSelenium', 'r')
myscript.readlines()

driver = webdriver.PhantomJS()
result = driver.execute_script(script)

driver.quit()
```

## Закрытие драйвера

Запуск драйвера  - процесс не быстрый. Для каждого открытия ссылки не надо запускать новый драйвер.

```
self.webdriver.quit()
```
