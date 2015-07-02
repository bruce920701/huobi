##1. Session management（会话管理）

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

