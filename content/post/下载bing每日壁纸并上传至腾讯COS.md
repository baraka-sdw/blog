---
title: 下载bing每日壁纸并上传至腾讯COS
date:  2018-05-17 21:45:35
categories: Python
tags: ["壁纸","爬虫"]
       
photos:
 - https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/bing_photos/20180517.jpg
---

<p class="description">每日一句:The great pleasure in life is doing what people say you cannot do.</p>

<!-- more -->

``` python
# -*- coding:utf-8 -*-

import urllib.request
import datetime
import json
from qcloud_cos import CosConfig
from qcloud_cos import CosS3Client
from qcloud_cos import CosServiceError
from qcloud_cos import CosClientError
import sys
import logging

# 腾讯云COSV5Python SDK, 目前可以支持Python2.6与Python2.7以及Python3.x

# pip安装指南:pip install -U cos-python-sdk-v5

# cos最新可用地域,参照https://www.qcloud.com/document/product/436/6224

logging.basicConfig(level=logging.INFO, stream=sys.stdout)

# 设置用户属性, 包括secret_id, secret_key, region
# appid已在配置中移除,请在参数Bucket中带上appid。Bucket由bucketname-appid组成
secret_id = ''     # 替换为用户的secret_id
secret_key = ''     # 替换为用户的secret_key
region = 'ap-chengdu'    # 替换为用户的region
token = ''                 # 使用临时秘钥需要传入Token，默认为空,可不填
config = CosConfig(Region=region, Secret_id=secret_id, Secret_key=secret_key, Token=token)  # 获取配置对象
client = CosS3Client(config)



# @brief  打开网页
# url : 网页地址
# @return 返回网页数据
def open_url(url):
    # 根据当前URL创建请求包
    req = urllib.request.Request(url)
    # 添加头信息，伪装成浏览器访问
    req.add_header('User-Agent',
                   'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/49.0.2623.112 Safari/537.36')
    # 发起请求
    response = urllib.request.urlopen(req)
    # 返回请求到的HTML信息
    return response.read()

# @brief 保存图片
# url : 图片url
# addr  : 保存的地址
def save_picture(url,addr):
    with open(addr, 'wb') as f:
        img = open_url(url)
        if img:
            f.write(img)
    print("图片已保存")
    return


for num in range(0,77):    # [1] 打开网页
    temp_str = "https://bing.ioliu.cn/v1/?d=+"+str(num)+"&w=1920&h=1080&type=json"
    response = open_url(temp_str)
    weatherJSON = json.loads(response)
    print (weatherJSON)
    # [3] 保存图片
    local_time_file_name = './bingphotos/'+weatherJSON['data']["enddate"] + ".jpg"
    print(local_time_file_name)
    save_picture(weatherJSON['data']["url"], local_time_file_name)

    file_name = './bing_photos/' + weatherJSON['data']["enddate"] + '.jpg'
    # 高级上传接口(推荐)
    response = client.upload_file(
        Bucket='sdw-1254060699',
        LocalFilePath='./bingphotos/' + weatherJSON['data']["enddate"] + '.jpg',
        Key=file_name,
        PartSize=10,
        MAXThread=10
    )
```