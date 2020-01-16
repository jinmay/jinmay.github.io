---
title: [selenium]셀레니움 사용시 특정 element의 클릭이 되지 않을때 대처법
---

셀레니움을 통해 특정 element를 클릭하려고 할때 보통은 아래와 같은 방법을 사용하곤 한다.

```python
body.find_element_by_class_name("btn_more").click()
```

click() 함수를 사용하게 되면 간단하게 해당 element를 클릭할 수 있는데, 원인 미상으로 가끔씩 클릭이 되지 않는 상황이 발생했다.

클릭을 구현하는 방법으로는 click() 함수와 submit() 함수를 사용할 수 있지만, 위의 상황처럼 보통의 방법으로는 클릭이 수행되지 않을때 아래의 코드 처럼 구현하면 문제 없이 사용할 수 있었다.

```python
from selenium.webdriver.common.keys import Keys

driver.find_element_by_id("TOTAL").send_keys(Keys.ENTER)
```

하지만,,  
위의 방법을 통해도 클릭이 되지 않을 수 있는데, 아래와 같은 방법을 시도해보자.

```python
element = driver.find_element_by_id("TOTAL")
driver.execute_script("arguments[0].click();", element)
```

---

[참고]  
https://wkdtjsgur100.github.io/selenium-does-not-work-to-click/
