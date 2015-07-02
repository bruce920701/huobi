##实时行情数据接口
###数据文件：
[BTC] `http://api.huobi.com/staticmarket/ticker_btc_json.js`

[LTC] `http://api.huobi.com/staticmarket/ticker_ltc_json.js`

###数据格式:
```javascript
{"ticker":{"high":86.48,"low":79.75,"last":83.9,"vol":2239560.1752883,"buy":83.88,"sell":83.9}}
```

##深度数据接口（json格式）
###数据文件
[BTC] `http://api.huobi.com/staticmarket/depth_btc_json.js`

[LTC] `http://api.huobi.com/staticmarket/depth_ltc_json.js`

指定深度数据条数（1-150条） 

[BTC] `http://api.huobi.com/staticmarket/depth_btc_X.js`

[LTC] `http://api.huobi.com/staticmarket/depth_ltc_X.js`

X表示返回多少条深度数据，可取值 1-150

###数据格式
```javascript
{"asks":[[90.8,0.5],...],"bids":[[86.06,79.243],...]]
```

卖:价格:累积量,...    买:价格:累积量...

