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
            md5(access_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx&amp;coin_type=1&amp;created=1386844119&amp;id=2&amp;method=order_info&amp;secret_key=xxxxxxxx-xxxxxxxx-xxxxxxxx-xxxxxxxx)
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

