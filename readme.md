# zp_stoken生成

可实现全自动完成zpstoken接口调用 无需补环境和手动注入代码
有需要可以联系v
anfengwork
本次boss分析zp_stoken  简单分析一下 思路，第一次发起请求后在response中会返回关键js参数进行拼接 关键js参数， 获取了几个关键参数，分别是此次加密算法执行的js文件名， 以及 加密算法的入参。接下来就是执行zp_stoken生成接口调用，此处会有浏览器环境模拟，请自行查看代码块。酌情测试！

Python请求实例代码

```
import requests
import json,time
def getToken_rpc(__zp_sseed__, __zp_sts__):
    while 1:
        try:
            data = {
                "group": "boss",
                "action": "data_encode",
                "param": json.dumps({"seed": __zp_sseed__, "ts": __zp_sts__})
            }
            res = requests.post('http://127.0.0.1:12080/go', data=data)
            data_rpc = res.json()['data']
            if data_rpc == '没有找到对应的group或clientId,请通过list接口查看现有的注入':
                print(f'暂无法连接 rpc zp_stoken接口，等待10秒... {data_rpc}')
                time.sleep(10)
            else:
                print(data_rpc)
                data_rpc = json.loads(data_rpc)
                zp_token = data_rpc['zp_token']
                user_agent = data_rpc['user-agent']
                print(f"rpc __zp_token__: {zp_token}")
                return zp_token
        except Exception:
            # print_exc()
            print('暂无法连接 rpc zp_stoken接口，等待10秒...')
            time.sleep(10)
```

