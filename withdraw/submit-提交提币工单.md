## [POST] /withdraw/submit.php

**请求：**

|参数      |类型   |说明                                                               |  
| --      |--     | --                                                                |
|appid    |string |用户身份ID, 一个唯一的随机字符串                                       |   
|cryptype |int    |0=data未加密(json格式)，1=data已加密(加密字符串)，当前只支持0            | 
|token    |string |md5("appid_salt_userid_timestamp")                      |
|timestamp|int    |时间戳,单位秒                                                        |
|subuserid|string |调用端子账号，字符串，平台不管其含义                                     |
|chain    |string |主链                                                                |
|coin     |string |币名                                                                |
|addr     |int    |提币目标地址                                                          |
|amount   |float  |提币数量                                                             |
|memo     |string |提币备注，主要用于备注用户ID（某些加密货币依赖备注进行入账,格式具体由接收方确定）|
|usertags|string |用户标签，用于提币异常时确定订单是否已存在，保证不重复调用该接口               |
|sign     |string |签名, md5("appid_salt_userid_timestamp_addr_memo_usertags"),如果memo、usertags字段为空，则字段前的下划线也要去掉  |


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
        "chain": "eth",     
        "coin": "usdt",     
        "addr": "",  
        "amount": 10,  
        "memo": "1234",
        "usertags": "1",
        "sign": "xxxxxxxx"   
    }
}
```

**注意： 扣减金额数量是 提币数量(amount) + (手续费数量)fee_amount**

**正常响应：**

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
|usertags          |string |用户标签，异常时确定订单使用,这里只是回传                       |
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
            "usertags": "",      
            "miner_coin": "",            
            "miner_amount": 0,          
            "time": ""     
        }
    }
}
```

**错误响应：(emsg中带有括号表示可能变动部分)**

|eno    |emsg                                |  description    |
| --    | --                                 |     --          |
|1      |system error (number)               |  系统内部错误     |
|2      |invalid json request                |  不是有效的json请求|
|3      |lack field: appid                   |  缺少appid字段   |
|4      |lack field: cryptype                |  缺少cryptype字段|
|5      |lack field: data                    |  缺少data字段    |
|6      |cryptype is invalid                 |  非法的cryptype字段|
|1001   |lack field: auth                    |  缺少auth字段    |
|3001   |lack field: token                   |  缺少token字段   |
|4001   |lack field: pubkey                  |  缺少pubkey字段  |
|5001   |lack field: timestamp               |  缺少timestamp字段|
|6001   |appid not found                     |  appid未找到    |
|7001   |timestamp difference over 30 seconds|  请求时间间隔超过30秒|
|8001   |appid is invalid                    |  非法的appid    |
|9001   |(name) not in ip whitelist          |  不受信任的IP地址 |
|10001  |token verifiy failed                |  token验证失败   |
|11001  |appid is disabled                   |  appid未启用     |
|1002   |(name) only support 'list' rule     |  规则定义错误     |
|2002   |(name) not string                   |  类型不是一个字符串 |
|3002   |invalid string                      |  非法的字符串     |
|4002   |function no implement               |  功能未实现       |
|5002   |function not support                |  功能不支持       |
|6002   |(name) not a list                   |  不是列表类型     |
|7002   |(name) cannot be empty              |  字段不能为空     |
|8002   |(name) not supported coin           |  不支持的币种     |
|9002   |(name) not supported chain          |  不支持的链       |
|10002  |(name) not positive int             |  不是一个正整数   |
|11002  |invalid withdraw addr (number)      |  非法的提币地址   |
|12002  |(name) not positive float           |  不是一个正浮点数 |
|16002  |(name) not float                    |  不是一个浮点型  |


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