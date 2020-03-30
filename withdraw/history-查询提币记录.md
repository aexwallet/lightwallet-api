## [POST] /withdraw/history.php 

**请求:**

|参数      |类型   |说明                                                     |  
| --      |--     | --                                                     |
|appid    |string |用户身份ID, 一个唯一的随机字符串                            |   
|cryptype |int    |0=data未加密(json格式)，1=data已加密(加密字符串)，当前只支持0 | 
|token    |string |md5("appid_salt_userid_timestamp")                     |
|timestamp|int    |时间戳,单位秒                                             |
|subuserid|string |调用端子账号，字符串，平台不管其含义                          |
|chain    |string |主链                                                    |
|coin     |string |币名                                                    |
|fromid   |int    |整数，从哪个充值序号开始，从0开始                           |
|limit    |int    |最多查询多少条记录，包含fromid这条记录                      |

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
        "subuserid": "", 
        "chain": "",      
        "coin": "",      
        "fromid": 12,   
        "limit": 100   
    }
}
```

**正常响应:**

|参数      |类型   |说明                                                                         |  
| --      |--     | --                                                                         |
|cryptype          |int    |0=data未加密(json格式)，1=data已加密(加密字符串)，当前只支持0         |   
|eno               |int    |0是正常，非0对应下面错误返回列表中的错误码                            | 
|emsg              |string |eno非0时对应下面错误返回列表中的错误描述                             |
|id                |int    |内部序号                                              |
|subuserid         |string |调用端子账号，字符串，平台不管其含义                          |
|chain             |string |主链                                                    |
|coin              |string |币名                                                    |
|from_addr         |string |发送地址(发送方)                                          |
|addr              |string |提币地址(接收方)                                           |
|amount            |float  |提币数量                                                  |
|amount_sent       |float  |实际发币数量                                               |
|sub_balance       |float  |余额扣减数量                                               |
|memo              |string |提币备注，可以是任意内容，一般备注用户id                       |
|status            |int    |提币状态: 1=准备发送,2=发送中,3=发送成功,4=发送失败,5=发送已取消 |
|status_desc       |string |提币状态文字说明                                            |
|txid              |string |交易ID                                                    |
|user_tags         |string |用户标签，异常时确定订单使用,回传                              |
|fee_coin          |string |手续费币种                                                 |
|fee_amount        |float  |手续费数量                                                 |
|miner_coin        |string |矿工费币种                                                 |
|miner_amount      |float  |矿工费数量                                                 |
|time              |string |订单创建时间                                               |


```json
字段值参考上表

{
    "cryptype": 0,  
    "data": {
        "eno":  0,  
        "emsg": "", 
        "data": [
            {
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
                "user_tags": "",      
                "miner_coin": "",            
                "miner_amount": 0,          
                "time": ""     
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
|9      |invalid finance id                  |
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