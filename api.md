<!-- TOC -->

- [通用](#通用)
- [首页](#首页)
    - [地图](#地图)
    - [湖北省详情](#湖北省详情)
    - [系统告警统计](#系统告警统计)
    - [湖北省概览信息](#湖北省概览信息)
    - [城市企业，网关分布](#城市企业网关分布)
    - [城市详情](#城市详情)
    - [城市列表](#城市列表)
- [总览](#总览)
    - [预测企业名](#预测企业名)
    - [安全生产](#安全生产)
    - [城市详情](#城市详情-1)
    - [城市安全生产](#城市安全生产)
    - [企业详情](#企业详情)
    - [企业网关设备](#企业网关设备)
- [网关设备](#网关设备)
    - [网关设备监控状态](#网关设备监控状态)
    - [报警信息](#报警信息)
    - [网关设备列表](#网关设备列表)
    - [返回网关设备密钥](#返回网关设备密钥)
    - [启用，禁用网关](#启用禁用网关)
    - [添加网关](#添加网关)
    - [检测网关名是否合法（网关名需要在一个企业内部是唯一的）](#检测网关名是否合法网关名需要在一个企业内部是唯一的)
    - [移除网关](#移除网关)
- [报警信息](#报警信息-1)
    - [报警信息列表](#报警信息列表)
    - [忽略报警,解决报警](#忽略报警解决报警)
- [用户](#用户)
    - [用户信息展示](#用户信息展示)
    - [手机号修改  用户修改密码](#手机号修改--用户修改密码)
    - [创建用户](#创建用户)
    - [用户列表](#用户列表)
    - [重置密码](#重置密码)
    - [删除用户](#删除用户)
    - [用户登陆](#用户登陆)

<!-- /TOC -->
# 通用
除登陆请求外的其他请求，应当在 request的header中设置 x-auth-token 字段，其值为登陆请求返回的token。 登陆请求会在response的header x-auth-token 中设置。

API前缀  /apis/

企业类型现在有6种
>工商贸企业
>烟花爆竹企业
>其他企业
>易制毒企业
>非煤矿山企业
>危险化学品企业

# 首页
## 地图
**url: /api/v1/homepage/map/**
> 首先调用城市列表接口返回所有的城市。当鼠标移动到某个城市上时，调用这个接口显示这个城市的具体信息。

POST

request
```
{
    "city_id": "201"
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
> CHANGE city_id 变成了字符串

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
    "gateway_online_percent": 96.0,
    "enterprise_count": 66640,
}
```

## 系统告警统计
**url: /api/v1/homepage/system-alarm/**

POST

request
```
{
    "filter": {
        "gateway_id": 10
    }
}
```

> 首页上不用传 filter 字段 \
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

旧的返回 response
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

```
{
    "connected_enterprise": 66640,
    "connected_city": 17,
    "grid": [
        {
            "connected_enterprise": 2991,
            "percent": 4,
            "city_name": "黄石市"
        },
        {
            "connected_enterprise": 6386,
            "percent": 9,
            "city_name": "襄阳市"
        },
        {
            "connected_enterprise": 25167,
            "percent": 37,
            "city_name": "武汉市"
        },
        {
            "connected_enterprise": 2722,
            "percent": 4,
            "city_name": "十堰市"
        },
        {
            "connected_enterprise": 5254,
            "percent": 7,
            "city_name": "宜昌市"
        },
        {
            "connected_enterprise": 1186,
            "percent": 1,
            "city_name": "随州市"
        },
        {
            "connected_enterprise": 2001,
            "percent": 3,
            "city_name": "咸宁市"
        },
        {
            "connected_enterprise": 625,
            "percent": 0,
            "city_name": "仙桃市"
        },
        {
            "connected_enterprise": 2642,
            "percent": 3,
            "city_name": "孝感市"
        },
        {
            "connected_enterprise": 8724,
            "percent": 13,
            "city_name": "荆州市"
        },
        {
            "connected_enterprise": 2452,
            "percent": 3,
            "city_name": "黄冈市"
        },
        {
            "connected_enterprise": 2384,
            "percent": 3,
            "city_name": "荆门市"
        },
        {
            "connected_enterprise": 1705,
            "percent": 2,
            "city_name": "恩施州"
        },
        {
            "connected_enterprise": 105,
            "percent": 0,
            "city_name": "神农架林区"
        },
        {
            "connected_enterprise": 603,
            "percent": 0,
            "city_name": "天门市"
        },
        {
            "connected_enterprise": 587,
            "percent": 0,
            "city_name": "潜江市"
        },
        {
            "connected_enterprise": 1106,
            "percent": 1,
            "city_name": "鄂州市"
        }
    ],
    "enterprise_type": {
        "危险化学品企业": 1873,
        "其他企业": 32748,
        "烟花爆竹企业": 1078,
        "非煤矿山企业": 1337,
        "工商贸企业": 29604
    }
}
```
 > CHANGE 新的返回 Response，主要在于enterprise_type 中直接返回中文，直接用来显示就行


## 城市企业，网关分布
**url: /api/v1/homepage/city-enterprise-dist/**

GET

response
```
[
    {
        "normal_rate": 0,
        "gateway_count": 0,
        "name": "武汉市",
        "enterprise_count": 25167
    },
    {
        "normal_rate": 0,
        "gateway_count": 0,
        "name": "神农架林区",
        "enterprise_count": 105
    },
    {
        "normal_rate": 0,
        "gateway_count": 0,
        "name": "十堰市",
        "enterprise_count": 2722
    },
    {
        "normal_rate": 0,
        "gateway_count": 1,
        "name": "黄石市",
        "enterprise_count": 2991
    },
    {
        "normal_rate": 0,
        "gateway_count": 0,
        "name": "随州市",
        "enterprise_count": 1186
    },
    {
        "normal_rate": 0,
        "gateway_count": 0,
        "name": "宜昌市",
        "enterprise_count": 5254
    },
    {
        "normal_rate": 0,
        "gateway_count": 0,
        "name": "黄冈市",
        "enterprise_count": 2452
    },
    {
        "normal_rate": 0,
        "gateway_count": 0,
        "name": "咸宁市",
        "enterprise_count": 2001
    },
    {
        "normal_rate": 0,
        "gateway_count": 0,
        "name": "荆州市",
        "enterprise_count": 8724
    },
    {
        "normal_rate": 0,
        "gateway_count": 0,
        "name": "孝感市",
        "enterprise_count": 2642
    },
    {
        "normal_rate": 0,
        "gateway_count": 0,
        "name": "鄂州市",
        "enterprise_count": 1106
    },
    {
        "normal_rate": 0,
        "gateway_count": 0,
        "name": "仙桃市",
        "enterprise_count": 625
    },
    {
        "normal_rate": 0,
        "gateway_count": 0,
        "name": "潜江市",
        "enterprise_count": 587
    },
    {
        "normal_rate": 0,
        "gateway_count": 0,
        "name": "天门市",
        "enterprise_count": 603
    },
    {
        "normal_rate": 0,
        "gateway_count": 0,
        "name": "荆门市",
        "enterprise_count": 2384
    },
    {
        "normal_rate": 0,
        "gateway_count": 0,
        "name": "襄阳市",
        "enterprise_count": 6386
    },
    {
        "normal_rate": 0,
        "gateway_count": 0,
        "name": "恩施州",
        "enterprise_count": 1705
    }
]

```

## 城市详情
**url: /api/v1/homepage/city-statistic/**

POST

request
```
{
    "city_id": "201"
}
```
> CHANGE 发送的id变成了字符串

response
```
{
    "connected_enterprise": 100,
    "enterprise_type": {
        "危险化学品企业": 1873,
        "其他企业": 32748,
        "烟花爆竹企业": 1078,
        "非煤矿山企业": 1337,
        "工商贸企业": 29604
    },
    "gateway_online_count": 80,
    "gateway_online_percent": 80.0
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
            "city_id": '201',
            "city_name": "武汉市"
        },
        {
            "city_id": '202',
            "city_name": "黄石市"
        } 
    ]
}
```
> 与之前的变化在于，city_id 返回的是字符串，之前返回的是整形

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
        "city_id": "201", // 
        "enterprise_type": "" //
    },
    "input": "水厂"
}
```
>企业类型现在有6种：
>工商贸企业
>烟花爆竹企业
>其他企业
>易制毒企业
>非煤矿山企业
>危险化学品企业
> CHANGE city_id 现在是字符串了

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
            "city_id": "1",
            "connected_enterprise": 100,
            "gateway_count": 100,
            "gateway_online_count": 30,
            "geteway_offline_count": 70
        }
    ]
}
```
> CHANGE city_id 返回字符串了

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
    },
    "city_id": "201"
}
> CHANGE city_id 变成了字符串

response
```
{
    "total_count": 100,
    "grid": [
        {
            "enterprise_name": "襄阳华润燃气",
            "enterprise_id": "fad5fc11202911e8b21700ff3c1702d7",
            "type": "危化品企业",
            "gateway_count": 10,
            "gateway_online_count": 3,
            "gateway_offline_count": 7
        }
    ]
}
```
> CHANGE 企业id变成了字符串

## 企业详情
**url: /api/v1/overview/enterprise/**

POST

request
```
{
    "enterprise_id": "fa48b550202911e8b21700ff3c1702d7"
}
```
> CHANGE 企业ID变成了字符串

response
```
{
    "enterprise_name": "襄阳华润煤气",
    "type": "危化品企业",
    "join_time": "2018-10-10 10:10:10", 
    "gateway_count": 10,
    "gateway_online_count": 3,
    "gateway_online_percent": 80
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
    },
    "city_id": "202",
    "enterprise_id": "fa48b550202911e8b21700ff3c1702d7"
}
```
>CHANGE city_id, enterprise_id 现在都是字符串

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
    "gateway_online_percent": 77
}
```

## 报警信息
> 但是这里的报警信息，只显示选中的网关设备的报警统计信息

## 网关设备列表
**url: /api/v1/gateway/gateway-list/**

POST

request
```
{
    "filter": {
        "page_size": 10,
        "page_number": 3,
        "city_id": "201",
        "gateway_status": "RUNNING",
        "gateway_name_or_enterprise_name": "襄阳" // 支持输入网关名和城市名
    }
}
```
> CHANGE city_id 变成了字符串

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
    "gateway_id": 12,
    "device_secret": "x2ksdf2nsdffsdf"
}
```


## 启用，禁用网关
**url: /api/v1/gateway/status/**

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
    "enterprise_id": "fa48b550202911e8b21700ff3c1702d7",
    "manufacturer": "华为",
    "desc": "机房西北角",
    "gateway_name": "网关1",
}
```
> CHANGE enterprise_id 变成字符串

response

## 检测网关名是否合法（网关名需要在一个企业内部是唯一的）
**url: /api/v1/gateway/is-name-avaliable/**

POST

request
```
{
    "enterprise_id": "fa48b550202911e8b21700ff3c1702d7",
    "gateway_name": "xxx",
}
```
> CHANGE enterprise_id 变成字符串

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
        "object": "GATEWAY",
        "city_id": "201",
        "status": "NOT_HANDLED",
        "begin_time": "2018-10-01",
        "end_time": "2018-10-03"
    }
}
```
> CHANGE city_id变成了字符串

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
**url: /api/v1/alarm/status/**

POST

request
```
{
    "alarm_id": 100, # 也支持列表传入一次对多个进行设置如： [100,101]
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
