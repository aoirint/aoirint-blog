---
canonical_url: ./
title: 'スニペット: docker-composeによるPython + Selenium環境'
# og_image:
# twitter_card: summary_large_image
og_description: 'docker-composeでPython + Seleniumを使える環境を整備する'
date: '2020-09-28 10:10:00'
draft: false
category: スニペット
tags:
  - Docker
  - Docker Compose
  - Python
  - Selenium
---
# スニペット: docker-composeによるPython + Selenium環境

docker-compose.yml
```dockercompose
version: '3.8'
services:
  app:
    build: ./app/
    entrypoint: [ "wait-for-it", "selenium:4444", "--", "python3", "/code/main.py" ]
    volumes:
      - ./work:/work
    environment:
      SELENIUM_URL: http://selenium:4444/wd/hub
    depends_on:
      - selenium
  selenium:
    image: selenium/standalone-chrome
    volumes:
      - /dev/shm:/dev/shm
```

app/Dockerfile
```dockerfile
FROM python:3

WORKDIR /work

RUN apt update && apt install -y \
  wait-for-it

ADD requirements.txt /tmp/
RUN pip3 install -r /tmp/requirements.txt

ADD code/ /code
```

app/code/main.py
```python
from selenium import webdriver
from selenium.common.exceptions import NoSuchElementException
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities

selenium_url = os.environ['SELENIUM_URL']

driver = webdriver.Remote(
    command_executor=selenium_url,
    desired_capabilities=DesiredCapabilities.CHROME,
)
driver_ua = driver.execute_script('return navigator.userAgent;')

driver.get(leaf_share_url)
```
