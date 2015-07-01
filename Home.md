### 交易 API 请求过程
1. 用户根据文档组织交易要素。

2. 通过POST方式提交交易请求。

3. `请求头信息中必须声明 Content-Type:application/x-www-form-urlencoded`

4. 火币网处理请求，并返回相应的JSON格式结果。

5. 交易API目前只支持https请求

6. apiV3 提交地址 https://api.huobi.com/apiv3 

7. 火币网API类似表单提交

8. 签名MD5必须是小写字母。
 
签名时的字符一律采用UTF-8编码。MD5散列后每个byte采用两个16进制小写字母高位在前低位在后表示。如 <br>
md5("比特币你好") = d6b6e11652b0c93c4f14cfb84c380541 <br>
md5("ABC123abc") = 85716f0702d2d464803e1366a7678d0b

### 代码示例
火币目前提供了JAVA、PHP、Python版本的代码示例，下载地址：https://github.com/huobiapi<br>
其他语言会根据需要相继支持。在使用过程中有任何问题请联系我们的API技术讨论QQ群，我们将在第一时间帮您解决技术问题。

以下是PHP代码示例
```php
<?php

// 密匙对
define('ACCESS_KEY', 'xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxx');
define('SECRET_KEY', 'xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxx');

// 使用 apiv3
define('API_URL', 'https://api.huobi.com/apiv3');

function httpRequest($pUrl, $pData){
	$tCh = curl_init();
	if($pData){
		is_array($pData) && $pData = http_build_query($pData);
		curl_setopt($tCh, CURLOPT_POST, true);
		curl_setopt($tCh, CURLOPT_POSTFIELDS, $pData);
	}
	curl_setopt($tCh, CURLOPT_HTTPHEADER, array("Content-type: application/x-www-form-urlencoded"));
	curl_setopt($tCh, CURLOPT_URL, $pUrl);
	curl_setopt($tCh, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($tCh, CURLOPT_SSL_VERIFYPEER, false);
	$tResult = curl_exec($tCh);
	curl_close($tCh);
	$tmp = json_decode($tResult, 1);
	if($tmp) {
		$tResult = $tmp;
	}
	return $tResult;
}

function createSign($pParams = array()){
	$pParams['secret_key'] = SECRET_KEY;
	ksort($pParams);
	$tPreSign = http_build_query($pParams);
	$tSign = md5($tPreSign);
	return strtolower($tSign);
}

function send2api($pParams, $extra = array()) {
	$pParams['access_key'] = ACCESS_KEY;
	$pParams['created'] = time();
	$pParams['sign'] = createSign($pParams);
	if($extra) {
		$pParams = array_merge($pParams, $extra);
	}
	$tResult = httpRequest(API_URL, $pParams);
	return $tResult;
}

function getAccountInfo(){
	$tParams = $extra = array();
	$tParams['method'] = 'get_account_info';
	// 不参与签名样例
	// $extra['test'] = 'test';
	$tResult = send2api($tParams, $extra);
	return $tResult;
}

try {
	var_dump(getAccountInfo());
} catch (Exception $e) {
	var_dump($e);
}
```
### 错误代码
`生成sign时，参与加密的参数按照a-z排序`

所有 API 方法调用在请求失败或遇到未知错误时会返回相关错误的JSON格式。

<table class="table table-bordered">
    <thead>
    <tr>
        <th>code</th>
        <th>message</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>1</th>
        <td>服务器错误</td>
    </tr>
    <tr>
        <th>2</th>
        <td>没有足够的人民币</td>
    </tr>
    <tr>
        <th>3</th>
        <td>交易已经开始，不能再次开始</td>
    </tr>
    <tr>
        <th>4</th>
        <td>交易已经结束</td>
    </tr>
    <tr>
        <th>5</th>
        <td>账户锁定</td>
    </tr>
    <tr>
        <th>10</th>
        <td>没有足够的比特币</td>
    </tr>
    <tr>
	    <th>11</th>
	    <td>没有足够的莱特币</td>
    </tr>
    <tr>
        <th>18</th>
        <td>资金密码错误</td>
    </tr>
    <tr>
        <th>26</th>
        <td>该委托不存在</td>
    </tr>
    <tr>
        <th>41</th>
        <td>该委托已经结束，不能修改</td>
    </tr>
    <tr>
        <th>42</th>
        <td>该委托已经被取消，不能修改</td>
    </tr>
    <tr>
        <th>44</th>
        <td>交易价钱太低</td>
    </tr>
    <tr>
        <th>45</th>
        <td>交易价格太高</td>
    </tr>
    <tr>
        <th>46</th>
        <td>交易数目太少,最少数量0.001</td>
    </tr>
    <tr>
        <th>47</th>
        <td>交易数目太多</td>
    </tr>
    <tr>
        <th>48</th>
        <td>买入金额不能小于1元</td>
    </tr>
    <tr>
        <th>49</th>
        <td>比特币买入数量不能小于0.001</td>
    </tr>
    <tr>
        <th>50</th>
        <td>莱特币买入价格不能高于现价的110%</td>
    </tr>
    <tr>
        <th>51</th>
        <td>莱特币买入数量不能小于0.01</td>
    </tr>
    <tr>
        <th>52</th>
        <td>比特币卖出数量不能小于0.001</td>
    </tr>
    <tr>
        <th>53</th>
        <td>莱特币卖出数量不能小于0.01</td>
    </tr>
    <tr>
        <th>54</th>
        <td>莱特币卖出价格不能低于现价的90%</td>
    </tr>
    <tr>
        <th>55</th>
        <td>比特币买入价格不能高于现价的110%</td>
    </tr>
    <tr>
        <th>56</th>
        <td>卖出价格不能低于现价的90%</td>
    </tr>
    <tr>
        <th>57</th>
        <td>卖出价格不能高于10万</td>
    </tr>
    <tr>
        <th>58</th>
        <td>买入价格不能高于现价的105%</td>
    </tr>
    <tr>
        <th>59</th>
        <td>卖出价格不能低于现价的95%</td>
    </tr>
    <tr>
        <th>63</th>
        <td>请求参数不正确</td>
    </tr>
    <tr>
        <th>64</th>
        <td>无效的请求</td>
    </tr>
    <tr>
        <th>65</th>
        <td>无效的方法</td>
    </tr>
    <tr>
        <th>66</th>
        <td>访问密匙验证失败</td>
    </tr>
    <tr>
        <th>67</th>
        <td>私钥验证失败</td>
    </tr>
    <tr>
        <th>68</th>
        <td>无效的价格</td>
    </tr>
    <tr>
        <th>69</th>
        <td>无效的数量</td>
    </tr>
    <tr>
        <th>70</th>
        <td>无效的提交时间</td>
    </tr>
    <tr>
        <th>71</th>
        <td>请求次数过多</td>
    </tr>
    <tr>
        <th>87</th>
        <td>交易数量小于 0.1 BTC，买入价格请不要高于市价 1%</td>
    </tr>
    <tr>
        <th>88</th>
        <td>交易数量小于 0.1 BTC，卖出价格请不要低于市价 1%</td>
    </tr>
    <tr>
        <th>89</th>
        <td>交易数量小于 0.1 LTC，买入价格请不要高于市价 1%</td>
    </tr>
    <tr>
        <th>90</th>
        <td>交易数量小于 0.1 LTC，卖出价格请不要低于市价 1%</td>
    </tr>
    <tr>
        <th>91</th>
        <td>无效的币种</td>
    </tr>
    <tr>
        <th>92</th>
        <td>买入价格不能高于现价的110%</td>
    </tr>
    <tr>
        <th>93</th>
        <td>卖出价格不能低于现价的90%</td>
    </tr>
    <tr>
        <th>97</th>
        <td>您已经开启交易资金密码，请提交资金密码参数</td>
    </tr>
    <tr>
        <th>107</th>
        <td>委托已经存在</td>
    </tr>
    <tr>
        <th>114</th>
        <td>交易金额不能多于10万元</td>
    </tr>
    <tr>
        <th>119</th>
        <td>比特币交易不能超过500个</td>
    </tr>
    <tr>
        <th>220</th>
        <td>莱特币交易不能超过20000个</td>
    </tr>
        <tr>
            <th>300</th>
            <td>非POST提交</td>
        </tr>
        <tr>
            <th>301</th>
            <td>未知币种</td>
        </tr>
        <tr>
            <th>302</th>
            <td>禁止提现</td>
        </tr>
        <tr>
            <th>303</th>
            <td>没有权限</td>
        </tr>
        <tr>
            <th>304</th>
            <td>用户锁定</td>
        </tr>
        <tr>
            <th>305</th>
            <td>资产异常，请联系客服</td>
        </tr>
        <tr>
            <th>306</th>
            <td>提现地址不存在</td>
        </tr>
        <tr>
            <th>307</th>
            <td>非安全认证提现地址</td>
        </tr>
        <tr>
            <th>308</th>
            <td>没有足够的比特币或莱特币,或者您申请了杠杆，目前提现额度为0</td>
        </tr>
        <tr>
            <th>309</th>
            <td>单笔提现比特币超过1000个,莱特币超过10000个时，请先与客服人员或客户经理预约提现</td>
        </tr>
        <tr>
            <th>310</th>
            <td>比特币最小提现额度为0.0100, 莱特币最小提现额度为0.1</td>
        </tr>
        <tr>
            <th>311</th>
            <td>低于提现手续费</td>
        </tr>
        <tr>
            <th>312</th>
            <td>超过当日累计</td>
        </tr>
        <tr>
            <th>313</th>
            <td>提币地址错误</td>
        </tr>
        <tr>
            <th>314</th>
            <td>提币记录不存在</td>
        </tr>
        <tr>
            <th>315</th>
            <td>用户信息不正确</td>
        </tr>
        <tr>
            <th>316</th>
            <td>该笔提现不能被取消</td>
        </tr>
        <tr>
            <th>317</th>
            <td>系统繁忙</td>
        </tr>
        <tr>
            <th>318</th>
            <td>未达到提现的安全标准</td>
        </tr>
    </tbody>
</table>

###API 方法
####获取个人资产信息 get_account_info
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>填写类型</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>method</th>
        <td>必填</td>
        <td>请求的方法 get_account_info</td>
    </tr>
    <tr>
        <th>access_key</th>
        <td>必填</td>
        <td>访问密匙</td>
    </tr>
    <tr>
        <th>created</th>
        <td>必填</td>
        <td>提交时间 10位时间戳</td>
    </tr>
    <tr>
        <th>sign</th>
        <td>必填</td>
        <td>MD5签名结果</td>
    </tr>
    <tr>
        <th>加密实例</th>
        <td colspan="2">sign =
            md5(access_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&created=1386844119&method=get_account_info&secret_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx)
        </td>
    </tr>
    </tbody>
</table>
####返回结果
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>total</th>
        <td>总资产折合</td>
    </tr>
    <tr>
        <th>net_asset</th>
        <td>净资产折合</td>
    </tr>
    <tr>
        <th>available_cny_display</th>
        <td>可用人民币</td>
    </tr>
    <tr>
        <th>available_btc_display</th>
        <td>可用比特币</td>
    </tr>
    <tr>
        <th>available_ltc_display</th>
        <td>可用莱特币</td>
    </tr>
    <tr>
        <th>frozen_cny_display</th>
        <td>冻结人民币</td>
    </tr>
    <tr>
        <th>frozen_btc_display</th>
        <td>冻结比特币</td>
    </tr>
    <tr>
        <th>frozen_ltc_display</th>
        <td>冻结莱特币</td>
    </tr>
    <tr>
        <th>loan_cny_display</th>
        <td>申请人民币数量</td>
    </tr>
    <tr>
        <th>loan_btc_display</th>
        <td>申请比特币数量</td>
    </tr>
    <tr>
        <th>loan_ltc_display</th>
        <td>申请莱特币数量</td>
    </tr>
    </tbody>
</table>

####获取所有正在进行的委托 get_orders
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>填写类型</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>method</th>
        <td>必填</td>
        <td>请求的方法 get_orders</td>
    </tr>
    <tr>
        <th>access_key</th>
        <td>必填</td>
        <td>访问密匙</td>
    </tr>
    <tr>
        <th>coin_type</th>
        <td>必填</td>
        <td>币种 1 比特币 2 莱特币</td>
    </tr>
    <tr>
        <th>created</th>
        <td>必填</td>
        <td>提交时间 10位时间戳</td>
    </tr>
    <tr>
        <th>sign</th>
        <td>必填</td>
        <td>MD5签名结果</td>
    </tr>
    <tr>
        <th>加密实例</th>
        <td colspan="2">sign =
            md5(access_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&coin_type=1&created=1386844119&method=get_orders&secret_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx)
        </td>
    </tr>
    </tbody>
</table>
####返回结果
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>id</th>
        <td>委托订单id</td>
    </tr>
    <tr>
        <th>type</th>
        <td>1买 2卖</td>
    </tr>
    <tr>
        <th>order_price</th>
        <td>委托价格</td>
    </tr>
    <tr>
        <th>order_amount</th>
        <td>委托数量</td>
    </tr>
    <tr>
        <th>processed_amount</th>
        <td>已经完成的数量</td>
    </tr>
    <tr>
        <th>order_time</th>
        <td>委托时间</td>
    </tr>
    </tbody>
</table>

####获取委托详情 order_info
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>填写类型</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>method</th>
        <td>必填</td>
        <td>请求的方法 order_info</td>
    </tr>
    <tr>
        <th>access_key</th>
        <td>必填</td>
        <td>访问密匙</td>
    </tr>
    <tr>
        <th>coin_type</th>
        <td>必填</td>
        <td>币种 1 比特币 2 莱特币</td>
    </tr>
    <tr>
        <td>id</td>
        <td>必填</td>
        <td>委托订单ID</td>
    </tr>
    <tr>
        <th>created</th>
        <td>必填</td>
        <td>提交时间 10位时间戳</td>
    </tr>
    <tr>
        <th>sign</th>
        <td>必填</td>
        <td>MD5签名结果</td>
    </tr>
    <tr>
        <th>加密实例</th>
        <td colspan="2">sign =
            md5(access_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&coin_type=1&created=1386844119&id=2&method=order_info&secret_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx)
        </td>
    </tr>
    </tbody>
</table>
####返回结果
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>id</th>
        <td>委托订单id</td>
    </tr>
    <tr>
        <th>type</th>
        <td>1买　2卖</td>
    </tr>
    <tr>
        <th>order_price</th>
        <td>委托价格</td>
    </tr>
    <tr>
        <th>order_amount</th>
        <td>委托数量</td>
    </tr>
    <tr>
        <th>processed_price</th>
        <td>成交平均价格</td>
    </tr>
    <tr>
        <th>processed_amount</th>
        <td>已经完成的数量</td>
    </tr>
    <tr>
        <td>vot</td>
        <td>交易额</td>
    </tr>
    <tr>
        <td>fee</td>
        <td>手续费</td>
    </tr>
    <tr>
        <td>total</td>
        <td>总交易额</td>
    </tr>
    <tr>
        <th>status</th>
        <td>状态　0未成交　1部分成交　2已完成　3已取消</td>
    </tr>
    </tbody>
</table>

####买入 buy
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>填写类型</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>method</th>
        <td>必填</td>
        <td>请求的方法 buy</td>
    </tr>
    <tr>
        <th>access_key</th>
        <td>必填</td>
        <td>访问密匙</td>
    </tr>
    <tr>
        <th>coin_type</th>
        <td>必填</td>
        <td>币种 1 比特币 2 莱特币</td>
    </tr>
    <tr>
        <th>price</th>
        <td>必填</td>
        <td>买入价格</td>
    </tr>
    <tr>
        <th>amount</th>
        <td>必填</td>
        <td>买入数量</td>
    </tr>
    <tr>
        <th>created</th>
        <td>必填</td>
        <td>提交时间 10位时间戳</td>
    </tr>
    <tr>
        <th>sign</th>
        <td>必填</td>
        <td>MD5签名结果</td>
    </tr>
    <tr>
        <th>加密实例</th>
        <td colspan="2">sign =
            md5(access_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&amount=10&coin_type=1&created=1386844119&method=buy&price=5000&secret_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx)
        </td>
    </tr>
    <tr>
        <th>trade_password</th>
        <td>选填</td>
        <td>此项不参与sign签名过程，如果开启下单时输入资金密码，必须传此参数</td>
    </tr>
    <tr>
        <th>trade_id</th>
        <td>选填</td>
        <td>此项不参与sign签名过程，用户自定义订单号为数字(最多15位，唯一值)</td>
    </tr>
    </tbody>
</table>
####返回结果
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>result</th>
        <td>成功状态 success</td>
    </tr>
    <tr>
        <th>id</th>
        <td>委托id</td>
    </tr>
    </tbody>
</table>

####卖出 sell
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>填写类型</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>method</th>
        <td>必填</td>
        <td>请求的方法 sell</td>
    </tr>
    <tr>
        <th>access_key</th>
        <td>必填</td>
        <td>访问密匙</td>
    </tr>
    <tr>
        <th>coin_type</th>
        <td>必填</td>
        <td>币种 1 比特币 2 莱特币</td>
    </tr>
    <tr>
        <th>price</th>
        <td>必填</td>
        <td>买入价格</td>
    </tr>
    <tr>
        <th>amount</th>
        <td>必填</td>
        <td>买入数量</td>
    </tr>
    <tr>
        <th>created</th>
        <td>必填</td>
        <td>提交时间 10位时间戳</td>
    </tr>
    <tr>
        <th>sign</th>
        <td>必填</td>
        <td>MD5签名结果</td>
    </tr>
    <tr>
        <th>加密实例</th>
        <td colspan="2">sign =
            md5(access_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&amount=10&coin_type=1&created=1386844119&method=sell&price=5000&secret_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx)
        </td>
    </tr>
    <tr>
        <th>trade_password</th>
        <td>选填</td>
        <td>此项不参与sign签名过程，如果开启下单时输入资金密码，必须传此参数</td>
    </tr>
    <tr>
        <th>trade_id</th>
        <td>选填</td>
        <td>此项不参与sign签名过程，用户自定义订单号为数字(最多15位，唯一值)</td>
    </tr>
    </tbody>
</table>
####返回结果
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>result</th>
        <td>成功状态 success</td>
    </tr>
    <tr>
        <th>id</th>
        <td>委托id</td>
    </tr>
    </tbody>
</table>

####买入(市价单) buy_market
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>填写类型</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>method</th>
        <td>必填</td>
        <td>请求的方法 buy_market</td>
    </tr>
    <tr>
        <th>access_key</th>
        <td>必填</td>
        <td>访问密匙</td>
    </tr>
    <tr>
        <th>coin_type</th>
        <td>必填</td>
        <td>币种 1 比特币 2 莱特币</td>
    </tr>
    <tr>
        <th>amount</th>
        <td>必填</td>
        <td>买入金额</td>
    </tr>
    <tr>
        <th>created</th>
        <td>必填</td>
        <td>提交时间 10位时间戳</td>
    </tr>
    <tr>
        <th>sign</th>
        <td>必填</td>
        <td>MD5签名结果</td>
    </tr>
    <tr>
        <th>加密实例</th>
        <td colspan="2">sign =
            md5(access_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&amount=10&coin_type=1&created=1386844119&method=buy_market&secret_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx)
        </td>
    </tr>
    <tr>
        <th>trade_password</th>
        <td>选填</td>
        <td>此项不参与sign签名过程，如果开启下单时输入资金密码，必须传此参数</td>
    </tr>
    <tr>
        <th>trade_id</th>
        <td>选填</td>
        <td>此项不参与sign签名过程，用户自定义订单号为数字(最多15位，唯一值)</td>
    </tr>
    </tbody>
</table>
####返回结果
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>result</th>
        <td>成功状态 success</td>
    </tr>
    <tr>
        <th>id</th>
        <td>委托id</td>
    </tr>
    </tbody>
</table>

####卖出(市价单) sell_market
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>填写类型</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>method</th>
        <td>必填</td>
        <td>请求的方法 sell_market</td>
    </tr>
    <tr>
        <th>access_key</th>
        <td>必填</td>
        <td>访问密匙</td>
    </tr>
    <tr>
        <th>coin_type</th>
        <td>必填</td>
        <td>币种 1 比特币 2 莱特币</td>
    </tr>
    <tr>
        <th>amount</th>
        <td>必填</td>
        <td>卖出数量</td>
    </tr>
    <tr>
        <th>created</th>
        <td>必填</td>
        <td>提交时间 10位时间戳</td>
    </tr>
    <tr>
        <th>sign</th>
        <td>必填</td>
        <td>MD5签名结果</td>
    </tr>
    <tr>
        <th>加密实例</th>
        <td colspan="2">sign =
            md5(access_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&amount=10&coin_type=1&created=1386844119&method=sell_market&secret_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx)
        </td>
    </tr>
    <tr>
        <th>trade_password</th>
        <td>选填</td>
        <td>此项不参与sign签名过程，如果开启下单时输入资金密码，必须传此参数</td>
    </tr>
    <tr>
        <th>trade_id</th>
        <td>选填</td>
        <td>此项不参与sign签名过程，用户自定义订单号为数字(最多15位，唯一值)</td>
    </tr>
    </tbody>
</table>
####返回结果
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>result</th>
        <td>成功状态 success</td>
    </tr>
    <tr>
        <th>id</th>
        <td>委托id</td>
    </tr>
    </tbody>
</table>

####取消委托单 cancel_order
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>填写类型</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>method</th>
        <td>必填</td>
        <td>请求的方法 cancel_order</td>
    </tr>
    <tr>
        <th>access_key</th>
        <td>必填</td>
        <td>访问密匙</td>
    </tr>
    <tr>
        <th>coin_type</th>
        <td>必填</td>
        <td>币种 1 比特币 2 莱特币</td>
    </tr>
    <tr>
        <th>id</th>
        <td>必填</td>
        <td>要取消的委托id</td>
    </tr>
    <tr>
        <th>created</th>
        <td>必填</td>
        <td>提交时间 10位时间戳</td>
    </tr>
    <tr>
        <th>sign</th>
        <td>必填</td>
        <td>MD5签名结果</td>
    </tr>
    <tr>
        <th>加密实例</th>
        <td colspan="2">sign = md5(access_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&coin_type=1&created=1386844119&id=1&method=cancel_order&secret_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx)
        </td>
    </tr>
    </tbody>
</table>
####返回结果
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>result</th>
        <td>成功状态 success</td>
    </tr>
    </tbody>
</table>
####修改订单 modify_order
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>填写类型</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>method</th>
        <td>必填</td>
        <td>请求的方法 modify_order</td>
    </tr>
    <tr>
        <th>access_key</th>
        <td>必填</td>
        <td>访问密匙</td>
    </tr>
    <tr>
        <th>coin_type</th>
        <td>必填</td>
        <td>币种 1 比特币 2 莱特币</td>
    </tr>
    <tr>
        <th>id</th>
        <td>必填</td>
        <td>委托单ID</td>
    </tr>
    <tr>
        <th>price</th>
        <td>必填</td>
        <td>下单价格</td>
    </tr>
    <tr>
        <th>amount</th>
        <td>必填</td>
        <td>下单数量</td>
    </tr>
    <tr>
        <th>created</th>
        <td>必填</td>
        <td>提交时间 10位时间戳</td>
    </tr>
    <tr>
        <th>sign</th>
        <td>必填</td>
        <td>MD5签名结果</td>
    </tr>
    <tr>
        <th>加密实例</th>
        <td colspan="2" style="word-break: break-all;">sign =
            md5(access_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&amount=10&coin_type=1&created=1386844119&id=98658&method=modify_order&price=5000&secret_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx)
        </td>
    </tr>
    </tbody>
</table>
####返回结果
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>result</th>
        <td>成功状态 success</td>
    </tr>
    <tr>
        <th>id</th>
        <td>新的委托id</td>
    </tr>
    </tbody>
</table>

####查询个人最新10条成交订单 get_new_deal_orders
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>填写类型</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>method</th>
        <td>必填</td>
        <td>请求的方法 get_new_deal_orders</td>
    </tr>
    <tr>
        <th>access_key</th>
        <td>必填</td>
        <td>访问密匙</td>
    </tr>
    <tr>
        <th>coin_type</th>
        <td>必填</td>
        <td>币种 1 比特币 2 莱特币</td>
    </tr>
    <tr>
        <th>created</th>
        <td>必填</td>
        <td>提交时间 10位时间戳</td>
    </tr>
    <tr>
        <th>sign</th>
        <td>必填</td>
        <td>MD5签名结果</td>
    </tr>
    <tr>
        <th>加密实例</th>
        <td colspan="2" style="word-break: break-all;">sign =
            md5(access_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&coin_type=1&created=1386844119&method=get_new_deal_orders&secret_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx)
        </td>
    </tr>
    </tbody>
</table>
####返回结果
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>id</th>
        <td>委托订单id</td>
    </tr>
    <tr>
        <th>type</th>
        <td>1买 2卖</td>
    </tr>
    <tr>
        <th>order_price</th>
        <td>委托价格</td>
    </tr>
    <tr>
        <th>order_amount</th>
        <td>委托数量</td>
    </tr>
    <tr>
        <th>processed_amount</th>
        <td>已经完成的数量</td>
    </tr>
    <tr>
        <th>last_processed_time</th>
        <td>最后成交时间</td>
    </tr>
    <tr>
        <th>order_time</th>
        <td>委托时间</td>
    </tr>
    <tr>
        <th>status</th>
        <td>状态　0未成交　1部分成交　2已完成　3已取消</td>
    </tr>
    </tbody>
</table>


####根据trade_id查询oder_id get_order_id_by_trade_id
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>填写类型</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>method</th>
        <td>必填</td>
        <td>请求的方法 get_order_id_by_trade_id</td>
    </tr>
    <tr>
        <th>access_key</th>
        <td>必填</td>
        <td>访问密匙</td>
    </tr>
    <tr>
        <th>coin_type</th>
        <td>必填</td>
        <td>币种 1 比特币 2 莱特币</td>
    </tr>
    <tr>
        <th>created</th>
        <td>必填</td>
        <td>提交时间 10位时间戳</td>
    </tr>
    <tr>
        <th>trade_id</th>
        <td>必填</td>
        <td>调用下单接口时的参数trade_id</td>
    </tr>
    <tr>
        <th>sign</th>
        <td>必填</td>
        <td>MD5签名结果</td>
    </tr>
    <tr>
        <th>加密实例</th>
        <td colspan="2" style="word-break: break-all;">sign = md5(access_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&coin_type=1&created=1386844119&method=get_order_id_by_trade_id&secret_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&trade_id=1)
        </td>
    </tr>
    </tbody>
</table>
####返回结果
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>order_id</th>
        <td>委托订单id</td>
    </tr>
    </tbody>
</table>


####提币BTC/LTC withdraw_coin
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>填写类型</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>method</th>
        <td>必填</td>
        <td>请求的方法 withdraw_coin</td>
    </tr>
    <tr>
        <th>access_key</th>
        <td>必填</td>
        <td>访问密匙</td>
    </tr>
    <tr>
        <th>coin_type</th>
        <td>必填</td>
        <td>币种 1 比特币 2 莱特币</td>
    </tr>
    <tr>
        <th>created</th>
        <td>必填</td>
        <td>提交时间 10位时间戳</td>
    </tr>
    <tr>
        <th>withdraw_address</th>
        <td>必填</td>
        <td>提币认证地址</td>
    </tr>
    <tr>
        <th>withdraw_amount</th>
        <td>必填</td>
        <td>提币数量 BTC&gt;=0.01 LTC&gt;=0.1</td>
    </tr>
    <tr>
        <th>sign</th>
        <td>必填</td>
        <td>MD5签名结果</td>
    </tr>
    <tr>
        <th>加密实例</th>
        <td colspan="2" style="word-break: break-all;">sign = md5(access_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&coin_type=1&created=1386844119&method=withdraw_coin&secret_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&withdraw_address=xxxxxx&withdraw_amount=0.1)
        </td>
    </tr>
    </tbody>
</table>
####返回结果
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>code</th>
        <td>正常200，失败时为错误代码</td>
    </tr>
    <tr>
        <th>message</th>
        <td>正常OK，失败时为提示信息</td>
    </tr>
    <tr>
        <th>withdraw_coin_id</th>
        <td>提币申请id</td>
    </tr>
    </tbody>
</table>


####取消提币BTC/LTC cancel_withdraw_coin
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>填写类型</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>method</th>
        <td>必填</td>
        <td>请求的方法 cancel_withdraw_coin</td>
    </tr>
    <tr>
        <th>access_key</th>
        <td>必填</td>
        <td>访问密匙</td>
    </tr>
    <tr>
        <th>created</th>
        <td>必填</td>
        <td>提交时间 10位时间戳</td>
    </tr>
    <tr>
        <th>withdraw_coin_id</th>
        <td>必填</td>
        <td>提币申请id</td>
    </tr>
    <tr>
        <th>sign</th>
        <td>必填</td>
        <td>MD5签名结果</td>
    </tr>
    <tr>
        <th>加密实例</th>
        <td colspan="2" style="word-break: break-all;">sign = md5(access_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&coin_type=1&created=1386844119&method=cancel_withdraw_coin&secret_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&withdraw_coin_id=123)
        </td>
    </tr>
    </tbody>
</table>
####返回结果
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>code</th>
        <td>正常200，失败时为错误代码</td>
    </tr>
    <tr>
        <th>message</th>
        <td>正常OK，失败时为提示信息</td>
    </tr>
    </tbody>
</table>


####查询提币BTC/LTC get_withdraw_coin_result
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>填写类型</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>method</th>
        <td>必填</td>
        <td>请求的方法 get_withdraw_coin_result</td>
    </tr>
    <tr>
        <th>access_key</th>
        <td>必填</td>
        <td>访问密匙</td>
    </tr>
    <tr>
        <th>created</th>
        <td>必填</td>
        <td>提交时间 10位时间戳</td>
    </tr>
    <tr>
        <th>withdraw_coin_id</th>
        <td>必填</td>
        <td>提币申请id</td>
    </tr>
    <tr>
        <th>sign</th>
        <td>必填</td>
        <td>MD5签名结果</td>
    </tr>
    <tr>
        <th>加密实例</th>
        <td colspan="2" style="word-break: break-all;">sign = md5(access_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&coin_type=1&created=1386844119&method=get_withdraw_coin_result&secret_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&withdraw_coin_id=123)
        </td>
    </tr>
    </tbody>
</table>
####返回结果
<table class="table table-bordered">
    <thead>
    <tr>
        <th>字段名</th>
        <th>描述</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th>code</th>
        <td>正常200，失败时为错误代码</td>
    </tr>
    <tr>
        <th>message</th>
        <td>正常OK，失败时为提示信息</td>
    </tr>
    <tr>
        <th>status_code</th>
        <td>提币状态码：1未审核，2审核通过，3处理中，4已汇出，5已取消，6失败</td>
    </tr>
    <tr>
        <th>status_message</th>
        <td>提币状态描述信息</td>
    </tr>
    </tbody>
</table>
