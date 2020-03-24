## [POST] /withdraw/status.php 

**请求：**

|参数        |类型   |说明                                                     |  
| --        |--     | --                                                     |
|appid      |string |用户身份ID, 一个唯一的随机字符串                            |   
|cryptype   |int    |0=data未加密(json格式)，1=data已加密(加密字符串)，当前只支持0 | 
|token      |string |md5("appid_salt_userid_timestamp")                     |
|timestamp  |int    |时间戳,单位秒                                             |
|coin       |string |币名                                                    |
|withdrawid |int    |提币订单ID                                               |

```json
字段值参考上表

{
    "appid": "", 
    "cryptype": 0,        
    "data": {
        "auth": {
            "token": "",  
            "timestamp": 1574151978    
        },
        "coin": "eth",  
        "withdrawid": 1 
    }
}
```

**正常响应：**

|参数      |类型   |说明                                                                         |  
| --      |--     | --                                                                         |
|cryptype              |int    |0=data未加密(json格式)，1=data已加密(加密字符串)，当前只支持0         |   
|eno                   |int    |0是正常，非0对应下面错误返回列表中的错误码                            | 
|emsg                  |string |eno非0时对应下面错误返回列表中的错误描述                             |
|id                    |int    |内部充值序号                                                     |
|subuserid             |string |调用端子账号，字符串，平台不管其含义                                 |
|chain                 |string |哪条主链上充值进来的                                              |
|coin                  |string |币名                                                            |
|addr                  |string |充值到哪个地址                                                   |
|amount                |float  |充值数量                                                        |
|amount_sent           |float  |实际发送的提币数量=(amount-fee_amount)                           |
|memo                  |string |提币备注，比如用户ID之类的，可以是任意内容                           |
|status                |int    |提币状态: 1=准备发送,2=发送中,3=发送成功,4=发送失败,5=发送已取消      |
|txid                  |string |链上的交易ID                                                   |
|fee_coin              |string |手续费币种                                                     |
|fee_amount            |float  |手续费数量                                                     |
|create_time           |string |订单创建时间                                                   |


```json
字段值参考上表

{
    "cryptype": 0,  
    "data": {
        "eno":  0,  
        "emsg": "", 
        "data": {
            "id": 23,     
            "subuserid": "1", 
            "chain": "eth",         
            "coin": "usdt",  
            "from_addr":"xxxxxx",       
            "addr": "xxxxxx",     
            "amount": 10,           
            "amount_sent": 9,       
            "sub_balance": 9,       
            "memo": "123",          
            "status": 0,            
            "status_desc": "",      
            "txid": "",                
            "fee_coin": "",            
            "fee_amount": 0,          
            "attch_id": 124,      
            "miner_coin": "",            
            "miner_amount": 0,          
            "create_time": ""           
        }
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
|9      |(返回具体错误)                        |
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
|8002   |(xxx) not supported coin            |
|10002  |invalid field value                 |

```json
字段值参考上表

{
    "cryptype": 0,  
    "data" : {
        "eno": "",          
        "emsg": "", 
        "data": {} 
    }
}
```
&nbsp;