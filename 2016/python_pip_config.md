# Configure PIP mirrors

Two configure files:

- ~/.pip/pip.conf, global
- $VIRTUAL_ENV/pip.conf, local

Two mirrors:

- http://pypi.douban.com/simple
- http://mirrors.aliyun.com/pypi/simple/

Configure syntax:

```
cat ~/.pip/pip.conf
[global]
index-url = http://pypi.douban.com/simple
trusted-host = pypi.douban.com
```

Refer:

- https://pip.pypa.io/en/stable/user_guide/#config-file

