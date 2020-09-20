# pytest_api_test
概要：对postman的一个接口做了接口自动化测试，使用Pytest+Request+Allure+Jenkins 框架
## 技术栈：
- python3 、pytest、Requests
- Allure
- Jenkins

## 测试接口介绍
本例使用Postman自带的一个接口来做测试，url如下：

```
https://postman-echo.com/time/before?timestamp=2016-10-10&target=2018-12-13
```
根据接口的请求参数timestamp、target和返回内容，大概设计了如下测试数据：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190716191837342.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dhbGVuMjAxNg==,size_16,color_FFFFFF,t_70)

## 项目介绍
1、本项目用Pycharm开发，新建一个项目后，还要添加如下依赖包：
- PyYAML
- allure-pytest
- requests
- selenium
- xlrd

2、config
这个包下放的配置文件，如环境、url地址等

3、test_case
这个包放的是测试用例文件，本次接口测试用例为：test_postman_api.py
另：conftest.py是pytest的一个pytest的配置文件，单独管理一些预置的操作场景

- 用例代码

```
# encoding: utf-8
import allure
import pytest
import requests
from utils.read_excel import *
'''
@author: luyihost
@file: test_postman_api.py
@time: 2019/7/8 20:54
@desc: 测试postman API
'''

test_data = get_xls()

@allure.feature('postman')
@allure.story('postman-api')
@pytest.mark.parametrize('timestamp,target,expected',test_data)
def test_timestamp(timestamp,target,expected,env_config):
    """
    用例描述：测试不同的timestamp和target
    """
    #从yml配置文件获取url
    url = env_config['host']['url']
    payload = {'timestamp':timestamp,'target':target}
    r = requests.get(url,params=payload)
    print(r.url)
    result = r.json()
    assert str(result['before'])==expected


```
4、 testdata
把测试数据放到excel里，实现脚本和数据分离

5、utils
工具类包，如读取excel文件

6、test_all_run.py
全局运行文件，实现运行所有测试用例


## Jenkins运行
可参考我的CSDN文档
https://blog.csdn.net/m0_48133963/article/details/108547777




