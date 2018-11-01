[TOC]

# 通用
除登陆请求外的其他请求，应当在 request的header中设置 x-auth-token 字段，其值为登陆请求返回的token。 登陆请求会在response的header x-auth-token 中设置。

# 首页
## 地图
**url: /api/v1/homepage/map/**
> 首先调用城市列表接口返回所有的城市。当鼠标移动到某个城市上时，调用这个接口显示这个城市的具体信息。

POST

request
```
{
    "city_id": 1
}
```

response
```
{
    "city_name": "武汉市",
    "connected_enterprise": 100,
    "gateway_count": 300,
    "gateway_online_count": 290,
    "gateway_offline_count": 10
}
```

## 湖北省详情
**url: /api/v1/homepage/prov-details/**

GET

request

response
```
{
    "connected_city": 10,
    "not_connected_city": 3,
    "city_count": 13,
    "gateway_count": 300,
    "gateway_online_count": 290,
    "online_percent": "96.0%"
}
```

## 系统告警统计
**url: /api/v1/homepage/system-alarm/**

POST

request
```
{
    "filter": {
        "object_type": "GATEWAY",
        "gateway_id": 10
    }
}
```

> object_type 包含两种类型，GATEWAY SENSOR 分别表示网关和传感器，首页上不用传 filter 字段 \
> 如果指定gateway_id 那么就只返回这个网关的告警信息，这个在网关设备页面会使用到

response
```
{
    "emergency": 3, //紧急
    "important": 5, //重要
    "normal": 9 //一般
}
```

## 湖北省概览信息
**url: /api/v1/homepage/prov-statistic/**

GET

request

response
```
{
    "connected_city": 10,
    "connected_enterprise": 100,
    "grid":  [
        {
            "city_name": "武汉",
            "connected_enterprise": 10,
            "percent": 10 //已接入城市占总接入城市的比例
        },
        {
            "city_name": "武汉",
            "connected_enterprise": 10,
            "percent": 10
        }
    ],
    "enterprise_type": {
        "meikuang": 3, //煤矿企业
        "yanhuabaozhu": 50, //烟花爆竹企业
        "weihuapin":  17, //危化品企业
        "kuangshan":, 30 //金属非金属企业
    },
}
```

## 城市详情
**url: /api/v1/homepage/city-statistic/**

POST

request
```
{
    "city_id": 1
}
```

response
```
{
    "connected_enterprise": 100,
    "enterprise_type": {
        "meikuang": 3, //煤矿企业
        "yanhuabaozhu": 50, //烟花爆竹企业
        "weihuapin":  17, //危化品企业
        "kuangshan":, 30 //金属非金属企业
    },
    "gateway_online_count": 80,
    "gateway_online_percent": "80.0%"
}
```
>  企业类型目前只有4中。web页面上应该显示其对应的中文

## 城市列表
**url: /api/v1/citys/**

GET

request

response
```
{
    "grid": [
        {
            "city_id": 1,
            "city_name": "武汉市"
        },
        {
            "city_id": 2,
            "city_name": "黄石市"
        } 
    ]
}
```

# 总览
> 树形图上方的企业搜索接口,可模糊匹配企业名，然后跳转到指定的企业页面

## 预测企业名
**url: /api/v1/enterprise/predict/**
> 用户输入的部分去做企业名匹配\
> 最多返回10个候选项\
> 添加网关设备的时偶，也会有模糊匹配企业名的需求，不过那里需要指定城市id。这里不用指定城市id，request中不包含filter即可。

POST

request
```
{
    "filter": {
        "city_id": 1, // 可填可不填
    },
    "input": "水厂"
}
```

response
```
{
    "grid":  [
        {
            "enterprise_name": "武汉汉口水厂",
            "enterprise_id": 100,
        },
        {
            "enterprise_name": "武汉武昌水厂",
            "enterprise_id": 100,
        }
    ]
}
```

## 安全生产
**url: /api/v1/overview/statistics/**

POST

request
```
{
    "filter": {
        "page_size": 10,
        "page_number": 1,
        "city_name": "襄" // 模糊匹配城市名
    }
}
```

response
```
{
    "total_count": 100, //总的行数
    "grid": [
        {
            "city_name": "武汉市",
            "city_id": 1,
            "connected_enterprise": 100,
            "gateway_count": 100,
            "gateway_online_count": 30,
            "geteway_offline_count": 70
        }
    ]
}
```

## 城市详情
与首页上的城市详情返回的数据一致

## 城市安全生产
**url: /api/v1/overview/city-statistics/**

POST

request
{
    "filter": {
        "page_size": 10,
        "page_number": 1,
        "enterprise_name": "襄阳华润"
    }
}

response
```
{
    "total_count": 100,
    "grid": [
        {
            "enterprise_name": "襄阳华润燃气",
            "enterprise_id": 200,
            "type": "危化品企业",
            "gateway_count": 10,
            "gateway_online_count": 3,
            "gateway_offline_count": 7
        }
    ]
}
```

## 企业详情
**url: /api/v1/overview/enterprise/**

POST

request
```
{
    "enterprise_id": 200
}
```

response
```
{
    "enterprise_name": "襄阳华润煤气",
    "type": "危化品企业",
    "join_time": "2018-10-10 10:10:10", 
    "gateway_count": 10,
    "gateway_online_count": 3,
    "gateway_online_percent": "80.8%"
}
```

## 企业网关设备
**url: /api/v1/overview/enterprise-gateway/**

POST

request
```
{
    "filter": {
        "page_size": 10,
        "page_number": 1,
        "gateway_name": "机房网关" //模糊匹配的gateway
    }
}
```

response
```
{
    "total_count": 100,
    "grid": [
        {
            "status": "RUNNING",
            "gateway_name": "机房网关",
            "manufacturer": "华为",
        }
    ]
}
```


# 网关设备

> 网关设备包括3中状态，RUNNING STOPPED ABNORMAL

## 网关设备监控状态
**url: /api/v1/gateway/statistics/**

GET

request

response
```
{
    "enterprise_count": 95,
    "gateway_count": 123,
    "gateway_online_count": 95
    "online_percent": 77
}
```

## 报警信息
> 复用首页的告警信息接口, 需要在filter中传入相应的 object_type = GATEWAY \
> 但是这里的报警信息，只显示网关设备状态的告警信息，不显示数据告警信息。

## 网关设备列表
**url: /api/v1/gateway/gateway-list/**

POST

request
```
{
    "filter": {
        "page_size": 10,
        "page_number": 3,
        "city_id": 1,
        "gateway_status": "RUNNING",
        "gateway_name_or_enterprise_name": "襄阳" // 支持输入网关名和城市名
    }
}
```

response
```
{
    "total_count": 100,
    "grid": [
        {
            "gateway_id": 12,
            "gateway_status": "RUNNING",
            "gateway_name": "机房网关",
            "city_name": "黄冈市",
            "enterprise_name": "黄冈水厂",
            "manufacturer": "华为",
            "throughput": "10M/s",
            "desc": "机房1，西边网关",
            "enabled": true
        }
    ]
}
```

## 返回网关设备密钥
**url: /api/v1/gateway/secret/**
>id 传入gateway_id

POST

request
```
{
    "gateway_id": 12
}
```

response
```
{
    "device_secret": "x2ksdf2nsdffsdf"
}
```


## 启用，禁用网关
**url: /api/v1/gateway/**

POST

request
```
{
    "gateway_id": 12,
    "enabled": true 
}
```
> 将网关的状态设置成预期的状态\
> true表示启用，false表示禁用

response

## 添加网关
**url: /api/v1/gateway/**

POST

request
```
{
    "city_id": 1 ,
    "enterprise_id": 100,
    "manufacturer": "华为",
    "desc": "机房西北角",
    "gateway_name": "网关1",
}
```

response

## 检测网关名是否合法（网关名需要在一个企业内部是唯一的）
**url: /api/v1/gateway/is-name-avaliable/**

POST

request
```
{
    "city_id": 1,
    "enterprise_id": 200,
    "gateway_name": "xxx",
}
```

response
```
{
    "avaliable": true 
}
```
> true 表示网关名可用




## 移除网关
**url: /api/v1/gateway/**

DELETE

request
```
{
    "gateway_id": 12，
}
```

response

# 报警信息
## 报警信息列表
**url: /api/v1/alarm/**

POST

request
```
{
    "filter": {
        "page_size": 10,
        "page_number": 1,
        "level": "EMERGENCY", 
        "object": "GATEWAY"
        "city_id": 1,
        "status": "NOT_HANDLED",
        "begin_time": "2018-10-01",
        "end_time": "2018-10-03"
    }
}
```

> level 字段包括 EMERGENCY IMPORTANT NORMAL 3中级别。填空表示返回级别的告警信息\
> object 字段包括 GATEWAY SENSOR \
> status 字段包括 NOT_HANDLED HANDLED IGNORED

response
```
{
    "total_count": 1000,
    "grid": [
        {
            "alarm_id": 100,
            "level": "EMERGENCY",
            "enterprise_name": "xxxxxx",
            "object": "网关设备", 
            "gateway_name": "机房网关1",
            "city_name": "武汉市",
            "alarm_content": "网关报警",
            "alarm_time": "2018-10-10 10:10:10",
            "handle_desc": "-"
        }
    ]
}
```

## 忽略报警,解决报警
**url: /api/v1/alarm/**

POST

request
```
{
    "alarm_id": 100,
    "status": "HANDLED"
}
```
> status 可以包括 HANDLED IGNORED分别表示设置成已处理和忽略

response

# 用户
## 用户信息展示
**url: /api/v1/user/**

GET

request

response
```
{
    "name":"xxx",
    "mail":"xxx@bbb.com",
    "phone":"13888888888"
}
````

## 手机号修改  用户修改密码
**url: /api/v1/user/**

PUT

request
```
{
    "phone": "13699999999",
    "old_password": "xxxxxx",
    "new_password": "bbbbbb"
}
```
> 修改手机号就传入phone即可；修改密码需要传入新旧密码

response
```
{
    "errstr":""
}
```


## 创建用户
**url: /api/v1/user**

POST

request
```
{
    "name": "xxx",
    "password": "xxxxxxx",
    "mail": "xxx@qq.com",
    "phone": "13588888888",
    "role": "admin"
}
```
> role目前有两种: admin/normal， 分别代表管理员和普通用户

response
```
{
    "errstr":""
}
```


## 用户列表
**url: /api/v1/user/pagination/**

POST

request
```
{
    "page_size": 10,
    "page_number": 1
}
```
> page_number 从 1 开始计数

response
```
{
    "page_size": 10,
    "page_number": 1,
    "total_count": 30
    "grid": [
        {
            "id": 1,
            "name": "xxx",
            "role": "normal",
            "email": "xxx@qq.com",
            "phone": 13488888888,
            "create_time": "2018-01-01"
        }
    ]
}
```

## 重置密码
**url: /api/v1/user/reset/**

POST

request
```
{
    "name": "xxx",
    "password": "xxxxxx"
}
```
> 传入需要重置密码的用户名和新的用户密码

response


## 删除用户
**url: /api/v1/user/**
DELETE

request
```
{
    "name": "xxxx"
}
```

## 用户登陆
**url: /api/v1/user/login/**

POST

request
```
{
    "name": "aaa",
    "password": "xxxxxx",
}
```

response
> headers中会设置一个 x-auth-token, 由服务器生成，后续操作需要在request的 heaer中设置 x-auth-token, 进行校验

```
{
    "id": 1,
    "name": "xxx",
    "role": "normal",
    "email": "xxx@qq.com",
    "phone": 13488888888,
    "create_time": "2018-01-01"
}
```