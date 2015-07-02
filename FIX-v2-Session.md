##火币网 FIX API
###FIX协议介绍
FIX协议是适用于实时证券、金融电子交易开发的数据通信标准在各类参与者之间，包括投资经理、经纪人，买方、卖方建立起实时的电子化通讯协议。它提供了一个ssl加密消息传递的可靠数据流，为用户提供了一个可靠和快速的消息传输机制。火币网FIX协议实现了Fix4.4版本，完整支持了Fix4.4的市场行情和交易。
###火币网FIX 信息格式
一条FIX协议信息的基本格式是：《标准头》+《信息正文域》+《标准尾》每条信息都是由一系列带有〈标记(Tag)〉=〈值(Value)〉的域组成的。在每个域之间通过"&lt; &gt;"分开。除了一些特殊规定外，信息中的域可按照任意顺序排列。所有域在都以"定界符(|)"表示终止。8=FIX.4.4|9=144|35=V|34=2|49=234abcxyzc|52=20140411-13:49:41.295|56=EXCHANGE|262=md1397224181279| 263=1|264=100|146=1|55=BTC/CNY|267=6|269=4|269=5|269=7|269=8|269=9|269=B|10=249|
##火币网FIX详解</h4>
###1. Session management（会话管理）

####1.1 Logon(A)-登录消息

客户端和服务端ssl握手成功后，建立通信通道后，客户端自动发送服务端第一条消息是LOGON(A)消息，消息格式如下：

<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th width='20%'>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="4">Logon(A)</th>
			<th>HeartBtInt</th>
			<td>Y</td>
			<td>心跳频率;心跳消息的时间间隔应当在每一个消息发送后复位，即发送一个消息后，在间隔给定的时间内无其它消息发送则发送一个心跳消息
			</td>
		</tr>
		<tr>
			<th>ResetSeqNumFlag</th>
			<td>N</td>
			<td>声明序号是否重置</td>
		</tr>
		<tr>
			<th>Username</th>
			<td>Y</td>
			<td>accessKey（需要上官网申请）</td>
		</tr>
		<tr>
			<th>Password</th>
			<td>Y</td>
			<td>secretKey（需要上官网申请）</td>
		</tr>
	</tbody>
</table>
####1.2 Logout(5)-退出消息
如果服务器验证客户端用户身份非法，服务端将发送Logout消息给客户端，或者客户端请求退出，需要向服务端发送Logout请求消息。否则认为是任何方式将Socket连接关闭，视为非法关闭。

<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="2">Logout(5)</th>
			<th>Text</th>
			<td>N</td>
			<td>文本说明注销的原因</td>
		</tr>
		<tr>
			<th>LogoutReason</th>
			<td>N</td>
			<td>声明注销的原因的类型</td>
		</tr>
	</tbody>
</table>
####1.3 HeartBeat(0)- 心跳消息
当客户端和服务端建立Session通道后，如果FIX两端有一方，在[HeartBtInt]心跳频率内，未收发消息，则发送一个TestQuest(1)请求，如果对方任然未回复，则认为Session通道已断开。
<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="1">HeartBeat(0)</th>
			<th>TextReqID</th>
			<td>N</td>
			<td>HeartBeat请求ID</td>
		</tr>
	</tbody>
</table>
####1.4	 ResendRequest(2)- 重发请求消息
如果检测到消息间隙，不是期望值的时候，会调用Resend Request消息重新请求。
<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="1">ResendRequest(2)</th>
			<th>TextReqID</th>
			<td>N</td>
			<td>ResendRequest请求ID</td>
		</tr>
	</tbody>
</table>

###2. Trading Interface(交易接口)
####2.1. AccountInfoRequest(Z1000)- 获取账户信息消息
当发送Logon消息登录成功后，需要使用AccountInfoRequest(Z1000)发送请求账户信息，AccountInfoRequest(Z1000)为火币自定义消息类型，需要开发者扩充该消息类型。账户请求消息格式如下
<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="5">AccountInfoRequest(Z1000)</th>
			<th>Account</th>
			<td>Y</td>
			<td>accessKey</td>
		</tr>
		<tr>
			<th>AccReqID</th>
			<td>Y</td>
			<td>本次请求账户请求ID</td>
		</tr>
		<tr>
			<th>Symbol</th>
			<td>Y</td>
			<td>btc为比特币,ltc为莱特币</td>
		</tr>

	</tbody>
</table>
####2.2. AccountInfoResponse(Z1001)- 账户信息响应消息
Fix服务端接收到AccountInfoRequest(Z1000)请求消息，Fix服务端，通过AccountInfoResponse (Z1001)将账户信息返回给客户端。返回的账户响应消息格式如下：
<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="8">AccountInfoResponse(Z1001)</th>
			<th>Account</th>
			<td>Y</td>
			<td>accessKey</td>
		</tr>
		<tr>
			<th>AccReqID</th>
			<td>N</td>
			<td>消息请求ID</td>
		</tr>
		<tr>
			<th>Free_CNY</th>
			<td>N</td>
			<td>可用人民币</td>
		</tr>
		<tr>
			<th>Free_BTC</th>
			<td>N</td>
			<td>可用比特币</td>
		</tr>
		<tr>
			<th>Free_LTC</th>
			<td>N</td>
			<td>可用莱特币</td>
		</tr>
		<tr>
			<th>Freezed_CNY</th>
			<td>N</td>
			<td>冻结人民币</td>
		</tr>
		<tr>
			<th>Freezed_BTC</th>
			<td>N</td>
			<td>冻结BTC</td>
		</tr>
		<tr>
			<th>Freezed_LTC</th>
			<td>N</td>
			<td>冻结LTC</td>
		</tr>
	</tbody>
</table>

####2.3. NewOrderSingle(D)- 创建订单请求消息</b>
当发送Logon消息登录成功后，创建订单NewOrderSingle(D) 消息请求格式如下：
<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="11">NewOrderSingle(D)</th>
			<th>Account</th>
			<td>Y</td>
			<td>accessKey</td>
		</tr>
		<tr>
			<th>ClOrdID</th>
			<td>Y</td>
			<td>订单号</td>
		</tr>
		<tr>
			<th>MinQty</th>
			<td>Y</td>
			<td>订单数据量</td>
		</tr>
		<tr>
			<th>OrdType</th>
			<td>Y</td>
			<td>订单类型：<br>
				1=Market(市价)<br>
				2=Limit (限价)
			</td>
		</tr>
		<tr>
			<th>Price</th>
			<td>Y</td>
			<td>价格</td>
		</tr>
		<tr>
			<th>Side</th>
			<td>Y</td>
			<td>买卖方<br>
				1=Buy<br>
				2=Sell
			</td>
		</tr>
		<tr>
			<th>Symbol</th>
			<td>Y</td>
			<td>btc/cny 或ltc/cny</td>
		</tr>
		<tr>
			<th>TransactTime</th>
			<td>Y</td>
			<td>订单请求时间</td>
		</tr>

		<tr>
			<th>OrdType</th>
			<td>Y</td>
			<td>订单类型</td>
		</tr>
	</tbody>
</table>
####2.4. ACK/REJECT-ExecutionReport(8)-执行报告信息</b>
FiX服务端接收到NewOrderSingle(D)消息后，会返回给客户端一个ACK/REJECT（确认或者拒绝）ExecutionReport(8)（执行报告信息）。消息格式如下所示：
<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th width='20%'>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="14">ExecutionReport(8)</th>
			<th>Account</th>
			<td>Y</td>
			<td>accessKey</td>
		</tr>
		<tr>
			<th>ClOrdID</th>
			<td>Y</td>
			<td>下单时用户请求的订单号</td>
		</tr>
		<tr>
			<th>MinQty</th>
			<td>Y</td>
			<td>订单数据量</td>
		</tr>
		<tr>
			<th>OrderID</th>
			<td>Y</td>
			<td>火币网生成的订单ID</td>
		</tr>
		<tr>
			<th>Price</th>
			<td>N</td>
			<td>价格</td>
		</tr>
		<tr>
			<th>Side</th>
			<td>Y</td>
			<td>买卖方<br>
				1=Buy<br>
				2=Sell
			</td>
		</tr>
		<tr>
			<th>Symbol</th>
			<td>Y</td>
			<td>btc/cny 或ltc/cny</td>
		</tr>
		<tr>
			<th>TransactTime</th>
			<td>Y</td>
			<td>订单请求时间</td>
		</tr>
		<tr>
			<th>ExecType</th>
			<td>Y</td>
			<td>执行结果(如果是NEW表示执行成功，如果是REJECTED表示执行失败)</td>
		</tr>
		<tr>
			<th>ExecID</th>
			<td>Y</td>
			<td>执行标识(如果执行失败包含错误码)</td>
		</tr>
		<tr>
			<th>OrdStatus</th>
			<td>Y</td>
			<td>订单状态</td>
		</tr>
		<tr>
			<th>LeavesQty</th>
			<td>Y</td>
			<td>完成订单数</td>
		</tr>
		<tr>
			<th>CumQty</th>
			<td>Y</td>
			<td>订单数</td>
		</tr>
		<tr>
			<th>AvgPx</th>
			<td>Y</td>
			<td>平均价</td>
		</tr>
	</tbody>
</table>
####2.5. OrderCancelRequest(F)-订单撤销请求消息
用户需要撤销一个订单，需要发送撤销订单请求-OrderCancelRequest(F)消息。消息格式如下：
<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="7">OrderCancelRequest(F)</th>
			<th>ClOrdID</th>
			<td>Y</td>
			<td>新生成的ID号</td>
		</tr>
		<tr>
			<th>OrigClOrdID</th>
			<td>Y</td>
			<td>请求撤单的ID号</td>
		</tr>
		<tr>
			<th>Symbol</th>
			<td>Y</td>
			<td>btc/cny 或ltc/cny</td>
		</tr>
		<tr>
			<th>TransactTime</th>
			<td>Y</td>
			<td>撤销发送的时间</td>
		</tr>

		<tr>
			<th>Side</th>
			<td>Y</td>
			<td>1买入2卖出</td>
		</tr>
	</tbody>
</table>
####2.6. OrderCancelReject(9)- 订单撤销请求返回消息
服务器接受到客户端撤销订单请求，服务端将发送订单撤销请求返回消息，消息格式如下：
<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="6">OrderCancelReject(9)</th>
			<th>ClOrdID</th>
			<td>Y</td>
			<td>请求ID</td>
		</tr>
		<tr>
			<th>OrderID</th>
			<td>Y</td>
			<td>请求撤销的订单ID</td>
		</tr>

		<tr>
			<th>OrigClOrdID</th>
			<td>Y</td>
			<td>请求撤销的订单ID</td>
		</tr>
		<tr>
			<th>OrdStatus</th>
			<td>Y</td>
			<td>订单状态</td>
		</tr>
		<tr>
			<th>TransactTime</th>
			<td>Y</td>
			<td>拒绝发送的时间</td>
		</tr>
		<tr>
			<th>Text</th>
			<td>N</td>
			<td>备注</td>
		</tr>
	</tbody>
</table>

####2.7. Order Mass Status Request(AF)-批量订单状态查询消息
客户端发送Order Mass Status Request(AF)-订单状态查询消息，服务端将以增量的方式，返回用户当前有效的委托订单。
<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="5">Order Mass Status Request(AF)</th>
			<th>Account</th>
			<td>N</td>
			<td>accessKey</td>
		</tr>
		<tr>
			<th>MassStatusReqID</th>
			<td>Y</td>
			<td>请求ID</td>
		</tr>
		<tr>
			<th>MassStatusReqType</th>
			<td>Y</td>
			<td>批量请求的状态范围</td>
		</tr>

	</tbody>
</table>
####2.8. Execution Report(8)-报告订单状态消息
当客户端请求订单状态时，服务器端通过Execution Report(8) 返回订单报告的详细信息。消息格式如下：
<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="17">EXECUTION REPORT(8)</th>
			<th>Account</th>
			<td>Y</td>
			<td>accessKey</td>
		</tr>
		<tr>
			<th>CIOrdID</th>
			<td>Y</td>
			<td>提交委托单初始值</td>
		</tr>
		<tr>
			<th>OrderID</th>
			<td>Y</td>
			<td>订单号</td>
		</tr>
		<tr>
			<th>AvgPx</th>
			<td>Y</td>
			<td>均价</td>
		</tr>
		<tr>
			<th>CumQty</th>
			<td>Y</td>
			<td>到订单取消时总计执行数量</td>
		</tr>
		<tr>
			<th>OrderQty</th>
			<td>Y</td>
			<td>初始订单量</td>
		</tr>
		<tr>
			<th>OrdType</th>
			<td>Y</td>
			<td>复制初始订单的值</td>
		</tr>
		<tr>
			<th>Side</th>
			<td>Y</td>
			<td>买卖方<br>
				1=Buy<br>
				2=Sell
			</td>
		</tr>
		<tr>
			<th>OrdStatus</th>
			<td>Y</td>
			<td>订单状态</td>
		</tr>
		<tr>
			<th>Price</th>
			<td>N</td>
			<td>价格</td>
		</tr>
		<tr>
			<th>TransactTime</th>
			<td>N</td>
			<td>执行报告时间</td>
		</tr>
		<tr>
			<th>ExecID</th>
			<td>N</td>
			<td>执行消息标识</td>
		</tr>
		<tr>
			<th>ExecType</th>
			<td>Y</td>
			<td>报告描述</td>
		</tr>
		<tr>
			<th>LeavesQty</th>
			<td>Y</td>
			<td>执行的数量</td>
		</tr>
		<tr>
			<th>TotNumReports</th>
			<td>Y</td>
			<td>报告的总数量</td>
		</tr>
		<tr>
			<th>Text</th>
			<td>N</td>
			<td>备注</td>
		</tr>
	</tbody>
</table>
####2.9 OrderStatusRequest(H)-单笔订单状态查询消息
客户端发OrderStatusRequest(H)-单笔订单状态查询消息，服务端返回指定的订单信息。
<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="6">OrderStatusRequest(H)</th>
			<th>Account</th>
			<td>N</td>
			<td>accessKey</td>
		</tr>

		<tr>
			<th>ClOrdID</th>
			<td>Y</td>
			<td>需查询的订单ID</td>
		</tr>
		<tr>
			<th>Symbol</th>
			<td>Y</td>
			<td>btc比特币ltc莱特币</td>
		</tr>
		<tr>
			<th>Side</th>
			<td>Y</td>
			<td>1买入2卖出</td>
		</tr>

	</tbody>
</table>
####2.10	HuobiOrderInfoResponse(Z1003)-单笔订单状态查询返回结果
HuobiOrderInfoResponse(Z1003)服务器接收到OrderStatusRequest(H)正常情况下的返回结果。
<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th width='20%'>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="11">HuobiOrderInfoResponse(Z1003)</th>
			<th>Symbol</th>
			<td>N</td>
			<td>btc比特币ltc莱特币</td>
		</tr>
		<tr>
			<th>OrderID</th>
			<td>Y</td>
			<td>查询的订单ID</td>
		</tr>
		<tr>
			<th>Side</th>
			<td>N</td>
			<td>1买入2卖出</td>
		</tr>
		<tr>
			<th>Price</th>
			<td>Y</td>
			<td>价格</td>
		</tr>
		<tr>
			<th>OrdStatus</th>
			<td>Y</td>
			<td>订单状态(0未成交　1部分成交　2已完成　4已取消)</td>
		</tr>
		<tr>
			<th>Quantity</th>
			<td>Y</td>
			<td>订单数量</td>
		</tr>
		<tr>
			<th>ProcessedPrice</th>
			<td>Y</td>
			<td>成交平均价格</td>
		</tr>
		<tr>
			<th>ProcessedAmount</th>
			<td>Y</td>
			<td>已经完成的数量</td>
		</tr>
		<tr>
			<th>Vot</th>
			<td>Y</td>
			<td>交易额</td>
		</tr>
		<tr>
			<th>Fee</th>
			<td>Y</td>
			<td>手续费</td>
		</tr>
		<tr>
			<th>Total</th>
			<td>Y</td>
			<td>总交易额</td>
		</tr>
	</tbody>
</table>

###错误代码

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
	<th>26</th>
	<td>该委托不存在</td>
</tr>
<tr>
	<th>32</th>
	<td>金额不足，无法交易或提现</td>
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
	<th>95</th>
	<td>连续输错5次资金密码，24小时内不能提现</td>
</tr>
<tr>
	<th>96</th>
	<td>连续输错资金密码次数</td>
</tr>
<tr>
	<th>97</th>
	<td>请输入资金密码</td>
</tr>
<tr>
	<th>98</th>
	<td>该委托单不存在</td>
</tr>
<tr>
	<th>107</th>
	<td>委托已经存在</td>
</tr>
<tr>
	<th>110</th>
	<td>买入价格不能超过2位小数</td>
</tr>
<tr>
	<th>111</th>
	<td>买入数量不能超过4位小数</td>
</tr>
<tr>
	<th>112</th>
	<td>卖出价格不能超过2位小数</td>
</tr>
<tr>
	<th>113</th>
	<td>卖出数量不能超过4位小数</td>
</tr>
<tr>
	<th>114</th>
	<td>买入金额不能多于1000000元</td>
</tr>
<tr>
	<th>119</th>
	<td>比特币交易不能超过500个</td>
</tr>
<tr>
	<th>220</th>
	<td>莱特币交易不能超过20000个</td>
</tr>
</tbody>
</table>


###3. Market Data Interface(行情接口)
####3.1. MarketDataRequest(V)-行情订阅请求消息
用户需发送一个订阅行情数据请求，MarketDataRequest(V)。服务端根据用户订阅的事件给用户推送实时行情信息。MarketDataRequest(V)

行情订阅分为三种类型订阅:
1. Order book request （买卖盘订阅 ）   (NoMDEntryTypes&lt;267&gt;=2,MDEntryType&lt;269&gt; is 0,1)
2. Trade request   （实时交易订阅） (NoMDEntryTypes&lt;267&gt;=1,MDEntryType&lt;269&gt; is 2)
3. 24 hour ticker request （24 th ticker订阅）(NoMDEntryTypes&lt;267&gt;=6,MDEntryType&lt;269&gt; is 4,5,7,8,9,B)<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th width='20%'>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="7">MarketDataRequest(V)</th>
			<th>&lt;NoRelatedSym&gt;</th>
			<td>Y</td>
			<td>重复组 NoRelatedSym，一直为1</td>
		</tr>
		<tr>
			<th>Symbol</th>
			<td>Y</td>
			<td>btc/cny 或ltc/cny（火币自定义）</td>
		</tr>
		<tr>
			<th>MDReqID</th>
			<td>Y</td>
			<td>指配给该请求的唯一ID，或前一个MarketDataRequest的ID以关闭订阅</td>
		</tr>
		<tr>
			<th>SubscriptionRequestType</th>
			<td>Y</td>
			<td>0 = 快照模式<br>
				1 = 快照+更新（订阅）<br>
				2 = 禁用上一个快照 + 更新请求（取消订阅）
			</td>
		</tr>
		<tr>
			<th>MarketDepth</th>
			<td>Y</td>
			<td>只在订单更新请求时有用，其他情况忽略。<br>
				0 = 由服务器端确定每次推送的深度<br>
				N = (1-150)每次推送N个深度的数据
			</td>
		</tr>
		<tr>
			<th>&lt;NoMDEntryTypes&gt;</th>
			<td>Y</td>
			<td>重复组NoMDEntryTypes。请求中实体类别的数量</td>
		</tr>
		<tr>
			<th>MDEntryType</th>
			<td>Y</td>
			<td>注册websocket行情深度数据(marketDepthTop)时必须包含MDEntryType.BID、MDEntryType.OFFER		注册websocket交易明细数据(tradeDetail)时必须包含MDEntryType.TRADE		注册websocket市场概况数据(marketOverview)时必须包含MDEntryType.OPENING_PRICE、MDEntryType.CLOSING_PRICE、MDEntryType.TRADING_SESSION_HIGH_PRICE、MDEntryType.TRADING_SESSION_LOW_PRICE、MDEntryType.TRADING_SESSION_VWAP_PRICE、MDEntryType.TRADE_VOLUME</p></td>
		</tr>
	</tbody>
</table>
####3.2. MarketDataSnapshotFullRefresh(W)-行情刷新消息
发送了MarketDataRequest(V)订阅行情消息，服务将实时推送行情数据MarketDataSnapshotFullRefresh(W)消息，实时发送行数据。
<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="8">MarketDataSnapshotFullRefresh(W)</th>
			<th>OrigTime</th>
			<td>Y</td>
			<td>消息响应时间</td>
		</tr>
		<tr>
			<th>Symbol</th>
			<td>Y</td>
			<td>btc/cny 或ltc/cny</td>
		</tr>
		<tr>
			<th>MDReqID</th>
			<td>N</td>
			<td>请求ID</td>
		</tr>
		<tr>
			<th>&lt;NoMDEntries&gt;</th>
			<td>Y</td>
			<td>行情Group组</td>
		</tr>
		<tr>
			<th>WV</th>
			<td>Y</td>
			<td>0=Bid卖方<br>
				1=Offer买方<br>
				2=Trade成交<br>
				4=OpeningPrice<br>
				5=ClosingPrice<br>
				6=Trading Session High Price&lt;44&gt;(最高价)<br>
				7= Trading Session Low Price&lt;44&gt;<br>（最低价）<br>
				8= Trading Session VWAP Price&lt;44&gt;
			</td>
		</tr>
		<tr>
			<th>MDEntryPx</th>
			<td>N</td>
			<td>成交价格</td>
		</tr>
		<tr>
			<th>MDEntrySize</th>
			<td>N</td>
			<td>交易数量</td>
		</tr>
		<tr>
			<th>Side</th>
			<td>N</td>
			<td>买卖方<br>
				1=Buy<br>
				2=Sell
			</td>
		</tr>
	</tbody>
</table>

##4. 异常返回消息Reject(msgtype=3)
acceptor端出现任何异常都返回该类型的消息
<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="2">Reject(msgtype=3)</th>
			<th>RefSeqNum</th>
			<td>Y</td>
			<td>quickfix框架中发送消息的编号，自动生成</td>
		</tr>
		<tr>
			<th>Text</th>
			<td>Y</td>
			<td>异常内容</td>
		</tr>
	</tbody>
</table>

###API参考实例

####FIX服务地址（开发及试用服务器，专用通道及SSL证书请单独找客服申请）

FIX-行情：`106.38.234.75:5000`

FIX-交易：`106.38.234.75:5001`

####火币FixCA证书，客户端SSL-秘钥，火币Fix协议

[下载地址](https://news.huobi.com/download/huobifix_pack.zip)

####火币QuickFixJ客户端实例

[下载地址](https://news.huobi.com/download/huobi_quickfixj_client_demo.zip)
