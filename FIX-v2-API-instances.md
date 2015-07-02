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

