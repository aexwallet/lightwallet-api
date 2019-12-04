## [POST] /info.php

**请求：**

|参数      |类型   |说明                                                     |  
| --      |--     | --                                                     |
|appid    |string |用户身份ID, 一个唯一的随机字符串                            |   
|cryptype |int    |0=data未加密(json格式)，1=data已加密(加密字符串)，当前只支持0 | 
|token    |string |md5("appid_salt_userid_timestamp")                      |
|timestamp|int    |时间戳,单位秒                                             |

```json
字段值参考上表

{
    "appid": "", 
    "cryptype": 0,
    "data": {
        "auth": {
            "token": "", 
            "timestamp": 0
        }
    }
}
```

**正常响应：**

|参数      |类型   |说明                                                                    |  
| --      |--     | --                                                                    |
|cryptype              |int    |0=data未加密(json格式)，1=data已加密(加密字符串)，当前只支持0    |   
|eno                   |int    |0是正常，非0对应下面错误返回列表中的错误码                       | 
|emsg                  |string |eno非0时对应下面错误返回列表中的错误描述                        |
|chain                 |string |链名                                                      |
|coin                  |string |币名                                                      |
|coin_precision        |int    |币的精度,也就是该币支持多少位小数                              |
|min_withdraw_amount   |float  |最小提币数量                                                |
|deposit_enabled       |int    |充值是否启用: 1=启用,0=未启用                                 |
|withdraw_enabled      |int    |提币是否启用: 1=启用,0=未启用                                 |
|deposit_confirm_count |int    |充值入账确认数                                               |
|fee_coin              |string |手续费币种                                                  |
|fee_type              |int    |手续费类型: 0=固定数量,1=费率,2=混合(withdraw_amt*rate+amount) |
|fee_amount            |float  |固定手续费数量                                               |
|fee_rate              |float  |手续费率                                                    |
|need_memo             |int    |充值是否需要备注: 1=充值需要备注,0=充值不需要备注                 |

```json
字段值参考上表

{
    "cryptype": 0, 
    "data": {
        "eno":  0, 
        "emsg": "",
        "data": [
            {
                "chain": "",
                "coin": "", 
                "coin_precision": 2, 
                "min_withdraw_amount": 0,
                "deposit_enabled": 1,    
                "withdraw_enabled": 1,   
                "deposit_confirm_count": 3,
                "fee_coin": "",   
                "fee_type": 0,    
                "fee_amount": 0.1,  
                "fee_rate": 0.001,
                "need_memo": 1 
            }
        ]
    }
}
```

**错误响应：**


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
