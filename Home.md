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

<table>
    <tr>
        <th>code</th>
        <th>message</th>
    </tr>
    <tr>
</table>