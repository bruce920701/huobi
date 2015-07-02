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

##分时行情数据接口（K线）
###数据文件：
[BTC] `http://api.huobi.com/staticmarket/btc_kline_[period]_json.js`

[LTC] `http://api.huobi.com/staticmarket/ltc_kline_[period]_json.js`

<table class="table table-bordered">
    <thead>
    <tr>
        <th width='25%'>period 参数</th>
        <th>说明</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>001</th>
        <td>1分钟线</td>
    </tr>
    <tr>
        <th>005</th>
        <td>5分钟</td>
    </tr>
    <tr>
        <th>015</th>
        <td>15分钟</td>
    </tr>
    <tr>
        <th>030</th>
        <td>30分钟</td>
    </tr>
    <tr>
        <th>060</th>
        <td>60分钟</td>
    </tr>
    <tr>
        <th>100</th>
        <td>日线</td>
    </tr>
    <tr>
        <th>200</th>
        <td>周线</td>
    </tr>
    <tr>
        <th>300</th>
        <td>月线</td>
    </tr>
    <tr>
        <th>400</th>
        <td>年线</td>
    </tr>
    <tr>
    	<th>例如</th>
        <td>
            [BTC] http://api.huobi.com/staticmarket/btc_kline_005_json.js<br>
            [LTC] http://api.huobi.com/staticmarket/ltc_kline_005_json.js
        </td>
    </tr>
    </tbody>
</table>
###数据格式:（json）
```javascript
[["20140417095000000",3297,3305,3290,3303,386.3926],...]
```

日期时间，开盘价，最高价，最低价，收盘价，成交量
###用例：
![行情图例](https://static.huobi.com/img/help/market_help_img1.png)

##买卖盘实时成交数据
###数据文件：
[BTC] `http://api.huobi.com/staticmarket/detail_btc_json.js`

[LTC] `http://api.huobi.com/staticmarket/detail_ltc_json.js`

###数据格式:(json)：
```javascript
{
  amount: 63165 //成交量
  level: 86.999 //涨幅
  buys: Array[10] //买10
  p_high: 4410 //最高
  p_last: 4275 &nbsp;//收盘价
  p_low: 4250 //最低
  p_new: 4362 //最新
  p_open: 4275 //开盘
  sells: Array[10] //卖10
  top_buy: Array[5] //买5
  top_sell: Object //卖5 
  total: 273542407.24361 //总量（人民币） 
  trades: Array[15] //实时成交
}
```

买卖盘用例：

![买卖盘用例](https://static.huobi.com/img/help/market_help_img3.png)

实时成交用例:

![实时成交用例](https://static.huobi.com/img/help/market_help_img4.png)