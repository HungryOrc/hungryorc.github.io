---
title: "Pytest assertions about exceptions"
tags: [python, pytest]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: pytest_assertion_exception.html
folder: pytest
toc: false
---

## Pytest assertions about exceptions

```python
# content of test_sysexit.py
import pytest
def f():
    raise SystemExit(1)

def test_mytest():
    with pytest.raises(SystemExit):
        f()
```




## Reference

* [The writing and reporting of assertions in tests [pytest.org]](https://docs.pytest.org/en/latest/assert.html)

{% include links.html %}
