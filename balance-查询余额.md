## [POST] /balance.php

**请求：**

|参数      |类型   |说明                                                     |  
| --      |--     | --                                                     |
|appid    |string |用户身份ID, 一个唯一的随机字符串                            |   
|cryptype |int    |0=data未加密(json格式)，1=data已加密(加密字符串)，当前只支持0 | 
|token    |string |md5("appid_salt_userid_timestamp")                      |
|timestamp|int    |时间戳,单位秒                                             |
|coins    |array  |币名数组，元素为字符串                                     |

```json
字段值参考上表

{
    "appid": "",  
    "cryptype": 0,
    "data": {
        "auth": {
            "token": "",
            "timestamp": 0
        },
        "coins": [ 
            "usdt" 
        ]
    }
}
```

**正常响应：**

|参数      |类型   |说明                                                                    |  
| --      |--     | --                                                                    |
|cryptype              |int    |0=data未加密(json格式)，1=data已加密(加密字符串)，当前只支持0    |   
|eno                   |int    |0是正常，非0对应下面错误返回列表中的错误码                       | 
|emsg                  |string |eno非0时对应下面错误返回列表中的错误描述                        |
|coin                  |string |币名                                                      |
|balance               |float  |余额数量                                                   |
|as_cny                |float  |余额以cnc为单位表示的数量                                    |

``` json
字段值参考上表

{
    "cryptype": 0, 
    "data": {
        "eno": 0,   
        "emsg": "",
        "data": [
            {
                "coin": "usdt",  
                "balance": 1,    
                "as_cny": 7.15,  
            }
        ]
    }
}
```

**错误返回：**


|eno    |emsg                                |
| --    | --                                 |
|1      |system error (number)               |
|2      |invalid json request                |
|3      |lack field: appid                   |
|4      |lack field: cryptype                |
|5      |lack field: data                    |
|6      |cryptype is invalid                 |
|1001   |lack field: auth                    |
|3001   |lack field: token                   |
|4001   |lack field: pubkey                  |
|5001   |lack field: timestamp               |
|6001   |appid not found                     |
|7001   |timestamp difference over 30 seconds|
|8001   |invalid appid                       |
|9001   |not trusted ip addr                 |
|10001  |token verifiy failed                |
|11001  |appid is disabled                   |
|1002   |invalid request format              |
|2002   |field type error                    |
|3002   |invalid field                       |
|4002   |invalid request format              |
|5002   |invalid request format              |
|6002   |(xxx) not found                     |
|7002   |field cannot be empty               |
|8002   |(xxx) unsupported coin              |

```json
字段值参考上表

{
    "cryptype": 0,  
    "data" : {
        "eno": "表第一列",          
        "emsg": "表第二列", 
        "data": [] 
    }
}
```