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
            md5(access_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&amp;amount=10&amp;coin_type=1&amp;created=1386844119&amp;id=98658&amp;method=modify_order&amp;price=5000&amp;secret_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx)
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

