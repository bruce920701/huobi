##借贷 
###loan
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
        <td>请求的方法 loan</td>
    </tr>
    <tr>
        <th>access_key</th>
        <td>必填</td>
        <td>访问公匙</td>
    </tr>
    <tr>
        <th>amount</th>
        <td>必填</td>
        <td>借贷数量</td>
    </tr>
    <tr>
        <th>loan_type</th>
        <td>必填</td>
        <td>借贷类型：1 人民币 2 比特币 3 莱特币</td>
    </tr>
    <tr>
        <th>created</th>
        <td>必填</td>
        <td>请求时间 10位时间戳</td>
    </tr>
    <tr>
        <th>sign</th>
        <td>必填</td>
        <td>MD5签名结果</td>
    </tr>
    <tr>
        <th>加密实例</th>
        <td colspan="2">sign=md5(access_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx
&amount=1000&created=1386844119&loan_type=1&method=loan
&secret_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx)
        </td>
    </tr>
    <tr>
        <th>market</th>
        <td>选填</td>
        <td>此项不参与sign签名过程，交易市场(cny:人民币交易市场，usd:美元交易市场，默认是cny)</td>
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
        <td>借贷记录id</td>
    </tr>
    <tr>
        <th>result</th>
        <td>success 成功</td>
    </tr>
    </tbody>
</table>
